<script setup lang="ts">
import { ref } from "vue";
import LoginForm from "./components/LoginForm.vue";
import SurveyForm from "./components/SurveyForm.vue";
import SurveyComplete from "./components/SurveyComplete.vue";
import DarkModeToggle from "./components/DarkModeToggle.vue";

interface UserData {
  enumeratorName: string;
  supervisorName: string;
}

const currentView = ref<"login" | "survey" | "complete">("login");
const userData = ref<UserData | null>(null);
const surveyData = ref(null);

const handleLogin = (data: UserData) => {
  userData.value = data;
  currentView.value = "survey";
};

const handleSurveyComplete = (data: any) => {
  surveyData.value = data;
  currentView.value = "complete";
};

const handleNewSurvey = () => {
  currentView.value = "login";
  userData.value = null;
  surveyData.value = null;
};
</script>

<template>
  <div class="min-h-screen bg-background">
    <!-- Dark Mode Toggle - Fixed position -->
    <div class="fixed top-4 right-4 z-50">
      <DarkModeToggle />
    </div>

    <!-- Login View -->
    <div
      v-if="currentView === 'login'"
      class="flex min-h-screen items-center justify-center p-6"
    >
      <div class="w-full max-w-md">
        <LoginForm @login="handleLogin" />
      </div>
    </div>

    <!-- Survey View -->
    <div v-else-if="currentView === 'survey'" class="min-h-screen">
      <SurveyForm :user-data="userData!" @complete="handleSurveyComplete" />
    </div>

    <!-- Survey Complete View -->
    <div
      v-else-if="currentView === 'complete'"
      class="flex min-h-screen items-center justify-center p-6"
    >
      <div class="w-full max-w-md">
        <SurveyComplete
          :survey-data="surveyData"
          @new-survey="handleNewSurvey"
        />
      </div>
    </div>
  </div>
</template>
