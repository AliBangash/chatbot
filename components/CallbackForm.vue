<template>
  <div class="callback-form">
    <h2 class="heading-data">Schedule a Callback</h2>
    <form @submit.prevent="handleSubmit">
      <div>
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="name" required />
      </div>
      <div>
        <label for="email">Email:</label>
        <input type="email" id="email" v-model="email" required />
      </div>
      <div>
        <label for="mobile">Mobile:</label>
        <input type="tel" id="mobile" v-model="mobile" required />
      </div>
      <div>
        <label for="message">Message:</label>
        <textarea id="message" v-model="message" required></textarea>
      </div>
      <button type="submit">Submit</button>
      <button type="button" @click="closeForm">Cancel</button>
    </form>

    <!-- Popup Modal -->
    <div v-if="showPopup" class="popup">
      <div class="popup-content">
        <p>Your query has been forwarded. We will contact you soon.</p>
        <button @click="closePopup">Close</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const name = ref("");
const email = ref("");
const mobile = ref("");
const message = ref("");

const showPopup = ref(false);

const emit = defineEmits(["close-form"]);

const handleSubmit = async () => {
  await submitForm();
  showPopup.value = true; // Show the popup on successful form submission
};

const closeForm = () => {
  emit("close-form");
};

const closePopup = () => {
  showPopup.value = false;
  closeForm(); // Close the form after closing the popup
};

const submitForm = async () => {
  const formData = {
    name: name.value,
    email: email.value,
  };

  try {
    const response = await fetch("https://b544-116-71-162-92.ngrok-free.app/send-email", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(formData),
    });

    if (!response.ok) {
      throw new Error("Network response was not ok");
    }

    const result = await response.json();
    console.log("Form submitted successfully:", result);
  } catch (error) {
    console.error("There was a problem with the fetch operation:", error);
  }
};
</script>

<style scoped>
.callback-form {
  padding: 20px;
  background: white;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
h2.heading-data {
  color: #00408abf;
}
.callback-form h2 {
  margin-top: 0;
}

.callback-form div {
  margin-bottom: 10px;
}

.callback-form label {
  display: block;
  margin-bottom: 5px;
}

.callback-form input,
.callback-form textarea {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.callback-form button {
  margin-right: 10px;
  padding: 10px 20px;
  border: none;
  background-color: #00408abf;
  color: white;
  border-radius: 5px;
  cursor: pointer;
}

.callback-form button:hover {
  background-color: #003366;
}

.callback-form button[type="button"] {
  background-color: gray;
}

.callback-form button[type="button"]:hover {
  background-color: darkgray;
}

/* Popup Styles */
.popup {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  border-radius: 10px;
  width: 100vw;
  height: 100vh;
}

.popup-content {
  background: white;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
  width: 50%;
  font-size: 14px;
}

.popup-content p {
  margin-bottom: 20px;
}

.popup-content button {
  padding: 10px 20px;
  border: none;
  background-color: #00408abf;
  color: white;
  border-radius: 5px;
  cursor: pointer;
}

.popup-content button:hover {
  background-color: #003366;
}
</style>
