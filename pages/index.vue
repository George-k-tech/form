<template>
  <div>
    <div>
      <form
        accept-charset="UTF-8"
        @submit.prevent="handleSubmit"
      >
        <div>
          <label>Email address</label>
          <input
            type="email"
            v-model="email"
            class="form-control"
            placeholder="Email"
          >
        </div>
        <div>
          <label>Name</label>
          <input
            type="text"
            v-model="name"
            class="form-control"
            placeholder="Name"
            required="required"
          >
        </div>
        <div>
          <label>Message</label>
          <textarea
            type="text"
            v-model="message"
            class="form-control"
            placeholder="Message"
            required="required"
          ></textarea>
        </div>
        <button type="submit">send</button>
      </form>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    name: 'ContactForm',
    data() {
      return {
        form: {
          email: "",
          name: "",
          message: "",
        },
      };
    },
    methods: {
      handleSubmit: async function() {
        const formData = new FormData();

        for (let [key, value] of Object.entries(this.form)) {
          formData.append(key, value);
        }

        await axios
          .post("https://formeezy.com/api/v1/forms/623af8eb2befc70009728fbb/submissions", formData)
          .then(({ data }) => {
            const { redirect } = data;
            // Redirect used for reCAPTCHA and/or thank you page
            window.location.href = redirect;
          })
          .catch((e) => {
            window.location.href = e.response.data.redirect;
          });
      }
    }
  };
</script>