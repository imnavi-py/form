<template>
  <div id="app">
    <h1>اطلاعات متقاضی</h1>
    <h5>لطفا مشخصات خود را برای ثبت نام در دوره جامع بازیگری وارد نمایید و اگر پیشینه‌ای دارید در قسمت توضیحات وارد کنید</h5>
    <form @submit.prevent="submitForm">
      <input type="text" v-model="user.firstName" placeholder=" نام" required />
      <input type="text" v-model="user.lastName" placeholder=" نام خانوادگی" required />
      <input type="text" v-model="user.job" placeholder=" شغل" required />
      <input type="text" v-model="user.nationalId" placeholder=" کد ملی" required maxlength="10" />

      <DatePicker v-model="user.birthDate" :column="1" mode="single" placeholder=" تاریخ تولد" class="date-picker" />

      <textarea v-model="user.description" placeholder=" توضیحات" required></textarea>
      <button type="submit">ارسال داده</button>
    </form>
    <toast-container />
  </div>
</template>

<script>
import { useToast } from 'vue-toastification';
import DatePicker from '@alireza-ab/vue3-persian-datepicker';

export default {
  components: {
    DatePicker,
  },
  data() {
    return {
      user: {
        firstName: '',
        lastName: '',
        job: '',
        nationalId: '',
        birthDate: '',
        description: '',
      },
    };
  },
  setup() {
    const toast = useToast();
    return { toast };
  },
  methods: {
    async submitForm() {
      const transformedData = this.transformUserData(this.user);
      console.log("داده‌های ارسال شده به سرور:", transformedData);
      
      try {
        const response = await this.sendDataToServer(transformedData);
        const message = response.message || 'اطلاعات شما با موفقیت ثبت شد!'; 
        this.toast.success(`پاسخ سرور: ${message}`);
        
        // پاک‌سازی فیلدها پس از موفقیت
        this.resetForm();
      } catch (error) {
        this.toast.error(`خطا: ${error.message}`);
      }
    },
    transformUserData(user) {
      return {
        first_name: user.firstName,
        last_name: user.lastName,
        job: user.job,
        national_id: user.nationalId,
        birth_date: this.formatDate(user.birthDate),
      };
    },
    formatDate(dateString) {
      const date = new Date(dateString);
      return date.toISOString().split('T')[0];
    },
    async sendDataToServer(userData) {
      const response = await fetch('https://app.beh-zendegi.ir/api/userinfo/', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(userData),
      });

      // بررسی وضعیت پاسخ
      if (!response.ok) {
        throw new Error('خطا در ارسال داده‌ها به سرور');
      }

      const responseData = await response.json();
      console.log("پاسخ سرور:", responseData); // چاپ پاسخ سرور
      return responseData;
    },
    resetForm() {
      this.user.firstName = '';
      this.user.lastName = '';
      this.user.job = '';
      this.user.nationalId = '';
      this.user.birthDate = '';
      this.user.description = '';
    }
  },
};
</script>

<style scoped>
/* General styling */
#app {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: linear-gradient(to bottom right, #333, #555); /* رنگ تیره برای پس‌زمینه */
  color: #fff; /* رنگ متن سفید */
  direction: rtl;
  padding: 20px;
}

h1 {
  margin-bottom: 20px;
  color: #ffcc80; /* رنگ عنوان */
  font-family: 'Arial', sans-serif;
  font-size: 28px;
}

h5 {
  margin-bottom: 12px;
  color: #c1b6a7; /* رنگ عنوان */
  font-family: 'Arial', sans-serif;
  font-size: 15px;
}

/* Form styling */
form {
  background: rgba(255, 255, 255, 0.9); /* پس‌زمینه نیمه‌شفاف */
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
  max-width: 500px;
  width: 100%;
  direction: rtl;
}

/* Input and Textarea styling */
input, textarea, .date-picker {
  width: calc(100% - 40px);
  padding: 15px 20px;
  margin: 15px 0;
  border: 1px solid #ffb74d;
  border-radius: 8px;
  font-size: 1em;
  font-weight: bold;
  font-family: 'Arial', sans-serif;
  background: #fff3e0;
  color: #333;
  transition: border-color 0.3s ease, background-color 0.3s ease;
}

input:focus, textarea:focus, .date-picker:focus {
  outline: none;
  border-color: #f57c00;
  background-color: #ffffff;
}

/* Button styling */
button {
  width: 100%;
  padding: 15px;
  background-color: #ff8f00;
  border: none;
  border-radius: 8px;
  color: white;
  font-size: 1.2em;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

button:hover {
  background-color: #f57c00;
  transform: scale(1.02);
}

/* Additional elements styling */
textarea {
  resize: vertical;
  min-height: 100px;
}

/* Placeholder styling */
::placeholder {
  color: #999;
  font-style: italic;
}
</style>
