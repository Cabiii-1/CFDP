<script setup lang="ts">
import { computed } from 'vue'
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import { Button } from '@/components/ui/button'
import { Alert, AlertDescription } from '@/components/ui/alert'
import { Badge } from '@/components/ui/badge'
import { CheckCircleIcon, PrinterIcon } from 'lucide-vue-next'

const props = defineProps<{
  surveyData: any
}>()

const emits = defineEmits<{
  newSurvey: []
}>()

const surveyId = computed(() => {
  const timestamp = new Date(props.surveyData?.completedAt || Date.now()).getTime()
  return `CFDP-${timestamp.toString().slice(-8)}`
})

const completionTime = computed(() => {
  if (!props.surveyData?.completedAt) return 'Unknown'
  return new Date(props.surveyData.completedAt).toLocaleString()
})

const householdHeadName = computed(() => {
  const sectionB = props.surveyData?.sectionB
  if (!sectionB) return 'Unknown'
  return `${sectionB.firstName || ''} ${sectionB.middleName || ''} ${sectionB.lastName || ''}`.trim()
})

const totalMembers = computed(() => {
  return props.surveyData?.sectionC?.totalMembers || 0
})

const enumeratorName = computed(() => {
  return props.surveyData?.userData?.enumeratorName || 'Unknown'
})

const handlePrint = () => {
  window.print()
}

const handleNewSurvey = () => {
  emits('newSurvey')
}
</script>

<template>
  <div class="flex flex-col gap-6">
    <Card>
      <CardHeader class="text-center">
        <div class="mb-4">
          <div class="mx-auto flex h-16 w-16 items-center justify-center rounded-full bg-green-100">
            <CheckCircleIcon class="h-8 w-8 text-green-600" />
          </div>
        </div>
        <CardTitle class="text-2xl text-green-600">Survey Completed!</CardTitle>
      </CardHeader>
      <CardContent class="space-y-6">
        <!-- Success Message -->
        <Alert class="border-green-200 bg-green-50">
          <CheckCircleIcon class="h-4 w-4 text-green-600" />
          <AlertDescription class="text-green-800">
            Thank you for completing the CFDP Digital Questionnaire. 
            Your responses have been successfully recorded.
          </AlertDescription>
        </Alert>

        <!-- Survey Summary -->
        <div class="space-y-4">
          <h3 class="text-lg font-semibold">Survey Summary</h3>
          
          <div class="grid grid-cols-1 gap-3">
            <!-- Household Head -->
            <div class="flex justify-between items-center py-2 border-b">
              <span class="font-medium">Household Head:</span>
              <Badge variant="secondary">{{ householdHeadName }}</Badge>
            </div>

            <!-- Completion Time -->
            <div class="flex justify-between items-center py-2 border-b">
              <span class="font-medium">Completed At:</span>
              <Badge variant="outline">{{ completionTime }}</Badge>
            </div>

            <!-- Total Members -->
            <div class="flex justify-between items-center py-2 border-b">
              <span class="font-medium">Household Members:</span>
              <Badge>{{ totalMembers }} members</Badge>
            </div>

            <!-- Enumerator -->
            <div class="flex justify-between items-center py-2">
              <span class="font-medium">Enumerator:</span>
              <Badge variant="secondary">{{ enumeratorName }}</Badge>
            </div>
          </div>
        </div>

        <!-- Action Buttons -->
        <div class="flex flex-col gap-3">
          <Button 
            variant="outline" 
            @click="handlePrint"
            class="w-full flex items-center gap-2"
          >
            <PrinterIcon class="h-4 w-4" />
            Print Survey
          </Button>
          
          <Button 
            @click="handleNewSurvey"
            class="w-full"
          >
            Start New Survey
          </Button>
        </div>

        <!-- Survey ID Footer -->
        <div class="text-center pt-4 border-t">
          <p class="text-sm text-muted-foreground">
            Survey ID: <span class="font-mono">{{ surveyId }}</span>
          </p>
        </div>
      </CardContent>
    </Card>
  </div>
</template>

<style scoped>
@media print {
  .no-print {
    display: none !important;
  }
}
</style>