<script setup lang="ts">
import { ref } from "vue";
import { Button } from "@/components/ui/button";
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";

const enumeratorName = ref("");
const supervisorName = ref("");

const emits = defineEmits<{
  login: [{ enumeratorName: string; supervisorName: string }];
}>();

const handleLogin = () => {
  if (enumeratorName.value && supervisorName.value) {
    emits("login", {
      enumeratorName: enumeratorName.value,
      supervisorName: supervisorName.value,
    });
  }
};
</script>

<template>
  <div class="flex flex-col gap-6">
    <Card>
      <CardHeader class="text-center">
        <div class="mb-4">
          <img
            src="C:\Users\Cj\Desktop\cj_code\nine\Rotating_earth_(large).gif"
            alt="CFDP Logo"
            class="mx-auto h-16 w-16 rounded-lg bg-gray-200"
            onerror="this.style.display='none'"
          />
        </div>
        <CardTitle class="text-2xl">CFDP Digital Questionnaire</CardTitle>
        <CardDescription>
          Community-Based Forest Development Program<br />
          Enter your details below to begin the survey
        </CardDescription>
      </CardHeader>
      <CardContent>
        <form @submit.prevent="handleLogin">
          <div class="flex flex-col gap-6">
            <div class="grid gap-3">
              <Label for="enumerator">Enumerator Name</Label>
              <Input
                id="enumerator"
                v-model="enumeratorName"
                type="text"
                placeholder="Enter your full name"
                required
              />
            </div>
            <div class="grid gap-3">
              <Label for="supervisor">Supervisor Name</Label>
              <Input
                id="supervisor"
                v-model="supervisorName"
                type="text"
                placeholder="Enter supervisor's full name"
                required
              />
            </div>
            <Button type="submit" class="w-full"> Start Survey </Button>
          </div>
        </form>
      </CardContent>
    </Card>
  </div>
</template>
