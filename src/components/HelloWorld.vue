<template>
  <div id="app">
    <h1>فرم ثبت مشخصات کاربر</h1>
    <form @submit.prevent="submitForm">
      <input type="text" v-model="user.firstName" placeholder="نام" required />
      <input type="text" v-model="user.lastName" placeholder="نام خانوادگی" required />
      <input type="text" v-model="user.job" placeholder="شغل" required />
      <input type="text" v-model="user.nationalId" placeholder="کد ملی" required />

      <!-- استفاده از DatePicker -->
      <DatePicker v-model="user.birthDate" :format="'jYYYY/jMM/jDD'" placeholder="تاریخ تولد" />

      <textarea v-model="user.description" placeholder="توضیحات" required></textarea>
      <button type="submit">ارسال داده</button>
    </form>
  </div>
</template>

<script>
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
  methods: {
    submitForm() {
      console.log("User data submitted:", this.user);
    },
  },
};
</script>

<style scoped>
/* General styling */
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f9f9f9;
  color: #333;
}

h1 {
  text-align: center;
  color: #333;
  margin-bottom: 20px;
  font-size: 1.8em;
}

/* Form styling */
form {
  background: #ffffff;
  padding: 30px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  max-width: 500px;
  width: 100%;
}

/* Input and Textarea styling */
input, textarea, .date-picker {
  width: 100%;
  padding: 12px 15px;
  margin: 10px 0;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 1em;
  background: #f7f7f7;
  color: #333;
}

input:focus, textarea:focus, .date-picker:focus {
  outline: none;
  border-color: #42b983;
  background-color: #fff;
}

/* Button styling */
button {
  width: 100%;
  padding: 12px;
  background-color: #42b983;
  border: none;
  border-radius: 5px;
  color: white;
  font-size: 1em;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #36936f;
}

/* Additional elements styling */
textarea {
  resize: vertical;
  min-height: 80px;
}

::placeholder {
  color: #999;
}
</style>
