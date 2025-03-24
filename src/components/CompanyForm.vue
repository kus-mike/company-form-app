<template>
  <div class="container">
    <div class="logo-container">
      <a href="https://vite.dev" target="_blank">
        <img src="/vite.svg" class="logo" alt="Vite logo" />
      </a>
    </div>

    <div class="form-container">
      <h2>Asian Development Bank</h2>
      <form @submit.prevent="validateForm">
        <div v-for="field in formFields" :key="field.name" class="form-group">
          <label>{{ field.label }}</label>

          <input
            v-if="field.type === 'text' || field.type === 'number'"
            :type="field.type"
            v-model="form[field.name]"
            class="form-control"
            :class="{ error: errors[field.name] }"
          />

          <template v-if="field.type === 'select'">
            <select
              v-model="form[field.name]"
              class="form-control"
              :class="{ error: errors[field.name] }"
            >
              <option value="">Select {{ field.label }}</option>
              <option
                v-for="option in field.options"
                :key="option.value"
                :value="option.value"
              >
                {{ option.label }}
              </option>
            </select>
          </template>

          <Datepicker
            v-if="field.type === 'date'"
            v-model="form[field.name]"
            :min-date="new Date()"
            class="form-control"
            :class="{ error: errors[field.name] }"
          />

          <span v-if="errors[field.name]" class="error-text">
            {{ field.label }} is required
          </span>
        </div>

        <button type="submit" class="btn">Submit</button>
      </form>
    </div>
  </div>
</template>

<script>
import Datepicker from "@vuepic/vue-datepicker";
import "@vuepic/vue-datepicker/dist/main.css";

export default {
  components: { Datepicker },
  data() {
    return {
      form: {
        companyName: "",
        status: "",
        bookingDate: null,
        exposure: "",
      },
      errors: {},
      formFields: [
        { name: "companyName", label: "Company Name", type: "text" },
        {
          name: "status",
          label: "Status",
          type: "select",
          options: [
            { value: "Active", label: "Active" },
            { value: "Inactive", label: "Inactive" },
          ],
        },
        { name: "bookingDate", label: "Booking Date", type: "date" },
        { name: "exposure", label: "Exposure", type: "number" },
      ],
    };
  },
  methods: {
    validateForm() {
      this.errors = {};

      this.formFields.forEach((field) => {
        if (!this.form[field.name]) {
          this.errors[field.name] = true;
        }
      });

      if (Object.keys(this.errors).length === 0) {
        alert("Form submitted successfully!");
      }
    },
  },
};
</script>

<style>
.container {
  width: 100%;
  max-width: 450px;
  margin: auto;
  padding: 25px;
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 1px solid #ddd;
  border-radius: 10px;
  background-color: #fdfdfd;
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.2);
}

.logo-container {
  margin-bottom: 15px;
}

.logo {
  height: 65px;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.form-container {
  width: 100%;
  text-align: center;
}

.form-group {
  display: flex;
  flex-direction: column;
  margin-bottom: 18px;
  text-align: left;
  width: 100%;
}

.form-control {
  width: 100%;
  padding: 12px;
  margin-top: 6px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 15px;
  box-sizing: border-box;
  transition: border-color 0.3s ease;
}

.form-control:focus {
  border-color: #007bff;
  outline: none;
}

.error {
  border-color: red !important;
}

.error-text {
  color: red;
  font-size: 13px;
  margin-top: 4px;
}

.btn {
  width: 100%;
  padding: 12px;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  font-weight: bold;
  transition: background 0.3s ease;
}

.btn:hover {
  background: #0056b3;
}
</style>
