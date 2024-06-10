<template>
  <div class="chat-container">
    <ChatHeader @close-chat="handleClose" />

    <div class="chat-content">
      <p style="padding: 0 15px; color: #333;">
        Welcome to FocusMed! How can I assist you today?
      </p>
      <NuxtImg src="/images/logo.svg" alt="logo" class="logoimg" />
      <p style="padding: 10px 15px; color: #333; line-height: 1.5;" class="para-data">
        I can help with sales-related topics or connect you </br>to a sales
        representative if you need further assistance. </br>You can get started
        by selecting one of the following </br> topics that best describes your
        need.
      </p>

      <ul v-if="!showSubMenu && !selectedQuestion && !showProductSubMenu" id="questionsList">
        <ChatListItem v-for="question in questions" :key="question.id" :question="question" @show-answer="showAnswer" />
      </ul>

      <div v-if="showProductSubMenu && !selectedProductSubQuestion && !showCallbackForm">
        <p>
          I can help you understand how our products and
          services can help you. You can type your question
          or select one of the following options:
        </p>
        <ul id="productSubMenu" class="subMenu">
          <ChatListItem v-for="productSubQuestion in productSubQuestions" :key="productSubQuestion.id"
            :question="productSubQuestion" @show-answer="showProductSubAnswer" />
        </ul>
        <button @click="showMainMenu" class="back-button">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M19 11H7.83l5.59-5.59L12 4l-8 8 8 8 1.41-1.41L7.83 13H19v-2z" fill="currentColor" />
          </svg>
        </button>
      </div>

      <ul v-if="showSubMenu && !selectedSubQuestion" id="hfSubMenu" class="subMenu">
        <ChatListItem v-for="subQuestion in subQuestions" :key="subQuestion.id" :question="subQuestion"
          @show-answer="showSubAnswer" />
      </ul>
      <button v-if="showSubMenu && !selectedSubQuestion" @click="showMainMenu" class="back-button">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M19 11H7.83l5.59-5.59L12 4l-8 8 8 8 1.41-1.41L7.83 13H19v-2z" fill="currentColor" />
        </svg>
      </button>

      <div v-if="selectedQuestion && !showSubMenu && !showProductSubMenu" class="selected-question">
        <p @click="showAnswer(selectedQuestion.text)" class="question">{{ selectedQuestion.text }}</p>
      </div>

      <div v-if="selectedSubQuestion" class="selected-question">
        <p @click="showSubAnswer(selectedSubQuestion.text)" class="question">{{ selectedSubQuestion.text }}</p>
      </div>

      <div v-if="selectedProductSubQuestion && !showCallbackForm" class="selected-question">
        <p @click="showProductSubAnswer(selectedProductSubQuestion.text)" class="question">{{
          selectedProductSubQuestion.text }}</p>
      </div>

      <p id="answer" v-html="answer"></p>

      <div v-if="selectedQuestion || selectedSubQuestion || selectedProductSubQuestion" class="button-container">
        <button v-if="selectedSubQuestion" @click="showSubMenuAgain" class="back-button">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M19 11H7.83l5.59-5.59L12 4l-8 8 8 8 1.41-1.41L7.83 13H19v-2z" fill="currentColor" />
          </svg>
        </button>
        <button v-if="selectedProductSubQuestion && !showCallbackForm" @click="showProductSubMenuAgain"
          class="back-button">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M19 11H7.83l5.59-5.59L12 4l-8 8 8 8 1.41-1.41L7.83 13H19v-2z" fill="currentColor" />
          </svg>
        </button>
        <button v-if="selectedQuestion && !selectedSubQuestion && !selectedProductSubQuestion" @click="clearSelection"
          class="back-button">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M19 11H7.83l5.59-5.59L12 4l-8 8 8 8 1.41-1.41L7.83 13H19v-2z" fill="currentColor" />
          </svg>
        </button>
      </div>

      <div v-if="showCallbackForm">
        <CallbackForm @close-form="closeCallbackForm" />
      </div>

      <div id="customAnswerContainer">
        <div v-for="customAnswer in customAnswers" :key="customAnswer.id">
          <div :class="getClass(customAnswer.type)">{{ customAnswer.text }}</div>

        </div>
      </div>
    </div>

    <div class="input-container">
      <input type="text" id="customQuestion" placeholder="Ask a question" @keydown.enter="askCustomQuestion"
        v-model="customQuestion" />
      <button @click="askCustomQuestion" class="askbtn">
        <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path fill-rule="evenodd" clip-rule="evenodd"
            d="M1.44731 0.105619C1.07815 -0.0789902 0.6335 -0.0178267 0.327895 0.259598C0.0222903 0.537023 -0.0814707 0.973704 0.0666757 1.35895L2.61935 7.99696L0.0668605 14.6243C-0.0814777 15.0095 0.0220426 15.4462 0.327477 15.7238C0.632912 16.0013 1.07751 16.0628 1.44676 15.8784L15.439 8.89191C15.7779 8.72266 15.9921 8.3764 15.9922 7.99754C15.9924 7.61868 15.7784 7.27229 15.4395 7.10284L1.44731 0.105619ZM4.37769 6.99724L2.85886 3.04764L10.7568 6.99724H4.37769ZM4.37731 8.99724L2.85977 12.9374L10.751 8.99724H4.37731Z">
          </path>
        </svg>
      </button>
    </div>

    <ConfirmationModal v-if="showModal" @clear-chat="clearChat" @close-modal="closeModal" />
  </div>
</template>

<script setup>
import { ref } from "vue";
import ChatListItem from "~/components/ChatListItem.vue";
import ChatHeader from "~/components/ChatHeader.vue";
import ConfirmationModal from "~/components/ConfirmationModal.vue";
import CallbackForm from "~/components/CallbackForm.vue";

function getClass(type) {
  return type === "question"
    ? "customQuestion"
    : type === "answer"
    ? "thankYouMessage"
    : "";
}

const answer = ref("");
const customQuestion = ref("");
const showSubMenu = ref(false);
const showProductSubMenu = ref(false);
const selectedQuestion = ref(null);
const selectedSubQuestion = ref(null);
const selectedProductSubQuestion = ref(null);
const showModal = ref(false);
const showCallbackForm = ref(false);
const isFirst = ref(true);
const sessionId = ref("");
const questions = [
  { id: 1, text: "Who We Are?" },
  {
    id: 2,
    text: "Learn about FocusMed Products and Services",
  },
  { id: 3, text: "Learn about High-Frequency Therapy" },
];

const subQuestions = [
  { id: 1, text: "Why high frequencies?" },
  { id: 2, text: "Benefits" },
  { id: 3, text: "How does it work?" },
  { id: 4, text: "Clinical Studies" },
  { id: 5, text: "Media Gallery" },
  { id: 6, text: "Celebrity endorsement" },
];

const productSubQuestions = [
  { id: 1, text: "Physiotherapy - Pain Therapy" },
  { id: 2, text: "Dermatology - Vulnology" },
  { id: 3, text: "Schedule a Callback with a Sales Rep" },
];

const customAnswers = ref([]);

const answers = {
  "Who We Are?":
    'We are an Italian company producing innovative and <br>cutting-edge electro-medical devices. Our devices are <br>innovative tools for physiotherapy and vulnology. </br> For more details  <a href="https://www.focusmed.it/"target="_blank"> check here </a>',
  "Learn about FocusMed Products and Services":
    'I can help you understand how our products and <br>services can help you. You can type your question <br> or select one of the following options: <br> <br> Physiotherapy - <a href="https://www.focusmed.it/prodotto/pronexibus-plus/" target="_blank" aria-label="Learn more about Pain Therapy">Pain Therapy</a> <br> Dermatology - <a href="https://www.focusmed.it/prodotto/rinovacell-ferite-cutanee-difficili/" target="_blank" aria-label="Learn more about Vulnology">Vulnology</a> <br> <br> Schedule a Callback with a Sales Rep',
  "Learn about High-Frequency Therapy": "",
  "Why high frequencies?":
    'Our body is made up of millions of cells. These, very schematically, are systems that differentiate themselves, and their internal environment, from the external environment through the cell membrane. <a href="https://www.focusmed.it/tecnologia/" target="_blank">Read more</a>',
  Benefits:
    "1. Immediate results on all musculoskeletal pathologies.<br>" +
    "2. It does not require conductive creams or gels, unlike tecartherapy devices.<br>" +
    "3. It can be used on hair (atlas, pubis, chest).<br>" +
    "4. It can be used over prosthetic materials (plates, osteosynthesis devices).<br>" +
    "5. No mechanical work is required by the operator.<br>" +
    '6. Lightweight and easy to carry. <a href="https://www.focusmed.it/2021/03/06/trattamento-diatermia-capacitiva/" target="_blank">Read more</a>',
  "How does it work?":
    "The ProNexibus Plus ® generates controlled high frequencies at 2, 4 and 8 MHz. The radio frequencies emitted penetrate deep into the tissues and overcome the resistance offered by the body and cell membranes.<br>" +
    'The high frequencies <a href="https://www.focusmed.it/prodotto/pronexibus-plus/" target="_blank">Read more</a>',
  "Clinical Studies":
    "Our devices are built to provide a practical, fast and effective response in the fields of physiotherapy and vulnology or difficult skin wounds.<br>" +
    'Why do we say they are innovative? Who detected and processed and validated the results? <a href="https://www.focusmed.it/2021/02/25/diatermia-capacitiva-benefici/" target="_blank">Read more</a>',
  "Media Gallery":
    "The controlled high frequencies of the ProNexibus Plus® allow the effective treatment of any patient, from the young athlete to the elderly post-prosthetic patient (shortening healing times), with any musculoskeletal, traumatic-inflammatory, neuromuscular or rheumatological problem, even in phase acute, such as: " +
    '<div class="media-gallery">' +
    '<img src="/images/gallery.png" alt="Media Image 1" class="media-image"/>' +
    '<img src="/images/gallery1.png" alt="Media Image 2" class="media-image"/>' +
    '<img src="/images/gallery2.png" alt="Media Image 3" class="media-image"/>' +
    "</div>",
  "Celebrity endorsement":
    'What do our users think? What do customers think? How to use our high frequency devices? <a href="https://www.focusmed.it/testimonial/" target="_blank">Read more</a>',
  "Physiotherapy - Pain Therapy":
    'FocusMed offers advanced solutions in physiotherapy, particularly for pain therapy. Our devices are designed to provide quick and effective relief for a variety of musculoskeletal conditions. <a href="https://www.focusmed.it/prodotto/pronexibus-plus/" target="_blank">Learn more</a>',
  "Dermatology - Vulnology":
    'In the field of dermatology, FocusMed specializes in the treatment of difficult skin wounds. Our innovative devices help accelerate the healing process and improve patient outcomes. <a href="https://www.focusmed.it/prodotto/rinovacell-ferite-cutanee-difficili/" target="_blank">Learn more</a>',
};

const showAnswer = (question) => {
  if (question === "Learn about High-Frequency Therapy") {
    showSubMenu.value = true;
  } else if (question === "Learn about FocusMed Products and Services") {
    showProductSubMenu.value = true;
  } else {
    answer.value = answers[question] || "";
    showSubMenu.value = false;
    showProductSubMenu.value = false;
    selectedQuestion.value = questions.find((q) => q.text === question);
    selectedSubQuestion.value = null;
    selectedProductSubQuestion.value = null;
    // Hide other answers
    document
      .querySelectorAll(".customAnswer")
      .forEach((el) => (el.style.display = "none"));
    document
      .querySelectorAll(".thankYouMessage")
      .forEach((el) => (el.style.display = "none"));
  }
};

const showSubAnswer = (subQuestion) => {
  answer.value = answers[subQuestion] || "";
  selectedSubQuestion.value = subQuestions.find((q) => q.text === subQuestion);
  // Hide other answers
  document
    .querySelectorAll(".customAnswer")
    .forEach((el) => (el.style.display = "none"));
  document
    .querySelectorAll(".thankYouMessage")
    .forEach((el) => (el.style.display = "none"));
};

const showProductSubAnswer = (productSubQuestion) => {
  if (productSubQuestion === "Schedule a Callback with a Sales Rep") {
    showCallbackForm.value = true;
  } else {
    answer.value = answers[productSubQuestion] || "";
    selectedProductSubQuestion.value = productSubQuestions.find(
      (q) => q.text === productSubQuestion
    );
    // Hide other answers
    document
      .querySelectorAll(".customAnswer")
      .forEach((el) => (el.style.display = "none"));
    document
      .querySelectorAll(".thankYouMessage")
      .forEach((el) => (el.style.display = "none"));
  }
};

const showMainMenu = () => {
  showSubMenu.value = false;
  showProductSubMenu.value = false;
  selectedQuestion.value = null;
  selectedSubQuestion.value = null;
  selectedProductSubQuestion.value = null;
  answer.value = "";
};

const showSubMenuAgain = () => {
  selectedSubQuestion.value = null;
  answer.value = "";
};

const showProductSubMenuAgain = () => {
  selectedProductSubQuestion.value = null;
  showCallbackForm.value = false;
  answer.value = "";
};

const clearSelection = () => {
  selectedQuestion.value = null;
  selectedSubQuestion.value = null;
  selectedProductSubQuestion.value = null;
  showCallbackForm.value = false;
  answer.value = "";
};

const askCustomQuestion = () => {
  if (customQuestion.value.trim() !== "") {
    customAnswers.value.push({
      id: Date.now(),
      text: customQuestion.value,
      type: "question",
      class: "customQuestion",
    });
    const question = customQuestion.value;

    fetch("http://3.227.203.53:8000/bot", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        query: customQuestion.value,
        session_id: sessionId.value,
        is_first: isFirst.value,
      }),
    })
      .then((response) => {
        if (!response.ok) {
          throw new Error("Network response was not ok");
        }
        return response.json();
      })
      .then((result) => {
        // Assuming the response contains the submitted question data
        isFirst.value = false;
        sessionId.value = result.session_id;
        customAnswers.value.push({
          id: result.session_id, // Assuming the API returns an id
          text: result.text,
          type: "answer",
          class: "customQuestion",
        });

        // Clear the input field
      })
      .catch((error) => {
        console.error("There was a problem with the fetch operation:", error);
      });

    customQuestion.value = ""; // Clear the input field
  }
};

const resetFaq = () => {
  showSubMenu.value = false;
  showProductSubMenu.value = false;
  selectedQuestion.value = null;
  selectedSubQuestion.value = null;
  selectedProductSubQuestion.value = null;
  showCallbackForm.value = false;
  answer.value = "";
  customQuestion.value = "";
  customAnswers.value = [];
};

const openModal = () => {
  showModal.value = true;
  document.body.style.overflow = "hidden"; // Lock body scroll
};

const closeModal = () => {
  showModal.value = false;
  document.body.style.overflow = "auto"; // Unlock body scroll
};

const clearChat = () => {
  resetFaq();
  closeModal();
};

const handleClose = () => {
  if (
    !selectedQuestion.value &&
    !selectedSubQuestion.value &&
    !selectedProductSubQuestion.value &&
    customAnswers.value.length === 0
  ) {
    // Directly close the chatbox
    console.log("Chatbox closed");
    // Implement the logic to close the chatbox here
  } else {
    openModal();
  }
};

const closeCallbackForm = () => {
  showCallbackForm.value = false;
};
</script>

<style scoped>
.chat-container {
   position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

p.para-data {
  border: 1px solid #8080805c;
  border-radius: 12px;
}

.chat-content {
  flex: 1;
  overflow-y: auto;
  padding: 10px;
}

.chat-content p {
  font-size: 14px;
  line-height: 2;
  text-align: justify;
}

.input-container {
  display: flex;
  align-items: center;
  padding: 10px;
  background: white;
  position: sticky;
  bottom: 0;
  width: 93%;
  box-shadow: 0 -1px 5px rgba(0, 0, 0, 0.1);
}

#questionsList,
.subMenu {
  list-style: none;
  padding: 0;
  margin: 0;
}

.input-container svg {
  fill: gray;
}

#questionsList li,
.subMenu li,
.selected-question .question,
.customQuestion {
  padding: 8px;
  background-color: #7ec0ec;
  margin: 10px 5px;
  cursor: pointer;
  color: #fff;
  border-radius: 10px;
}

#questionsList li:hover,
.subMenu li:hover,
.selected-question .question:hover,
.customQuestion:hover {
  background-color: #7ec0ec9c;
}

#answer {
  margin-top: 10px;
  color: #333;
  padding: 0 15px 15px 15px;
  line-height: 1.5;
}

.media-image {
  width: 100%;
  object-fit: cover;
}

.media-image {
  width: 100%;
  object-fit: cover;
  margin: 10px 0;
}

.customQuestion {
  background-color: #7ec0ec;
  color: white;
  padding: 8px;
  border-radius: 10px;
  margin-bottom: 5px;
}

.thankYouMessage {
  margin-top: 5px;
  color: #333;
  padding: 8px;
  border-radius: 10px;
}

input#customQuestion {
  height: 30px;
  width: 100%;
  border-radius: 10px;
  padding: 0 10px;
  border: 1.5px solid #00408a;
}

button.askbtn {
  position: absolute;
  right: 3px;
  top: 19px;
}

button {
  background: none;
  border: none;
  cursor: pointer;
  padding: 0 10px;
}

.back-button {
  background-color: #7ec0ec;
  color: white;
  padding: 8px 16px;
  margin-top: 10px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.back-button svg {
  margin-right: 5px;
}

.back-button:hover {
  background-color: #7ec0ec9c;
}

.button-container {
  display: flex;
  margin-top: 10px;
}

.thankYouMessage {
  font-size: 14px;
  margin-top: 10px;
  text-align: justify;
  line-height: 1.5;
  background: #8080801a;
}

.logoimg {
  width: 50%;
  margin: 0 auto;
  display: flex;
}

.media-image {
  width: 100%;
  object-fit: cover;
  margin: 10px 0;
}

.schedule-callback {
  padding: 8px;
  background-color: #ffcc00;
  margin: 10px 5px;
  cursor: default;
  color: #000;
  border-radius: 10px;
  text-align: center;
  font-weight: bold;
}

.media-gallery {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 10px;
}

.media-image {
  width: calc(33% - 10px);
  border-radius: 10px;
}
</style>
