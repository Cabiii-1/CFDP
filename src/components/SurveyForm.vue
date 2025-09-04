<script setup lang="ts">
import { ref, computed } from 'vue'
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import { Tabs, TabsContent, TabsList, TabsTrigger } from '@/components/ui/tabs'
import { Progress } from '@/components/ui/progress'
import { Button } from '@/components/ui/button'
import SectionA from './sections/SectionA.vue'
import SectionB from './sections/SectionB.vue'
import SectionC from './sections/SectionC.vue'

interface UserData {
  enumeratorName: string
  supervisorName: string
}

const props = defineProps<{
  userData: UserData
}>()

const emits = defineEmits<{
  complete: [data: any]
}>()

const currentSection = ref('A')
const formData = ref({
  sectionA: {},
  sectionB: {},
  sectionC: {},
  sectionD: {},
  sectionE: {},
  sectionF: {},
  sectionG: {},
  sectionH: {},
  sectionI: {},
  sectionJ: {},
  sectionK: {}
})

const sections = [
  { id: 'A', title: 'Household Identification', component: 'SectionA' },
  { id: 'B', title: 'Head of Family Profile', component: 'SectionB' },
  { id: 'C', title: 'Household Composition', component: 'SectionC' },
  { id: 'D', title: 'Housing & Living Conditions', component: 'SectionD' },
  { id: 'E', title: 'Health & Nutrition', component: 'SectionE' },
  { id: 'F', title: 'Education', component: 'SectionF' },
  { id: 'G', title: 'Livelihood & Income', component: 'SectionG' },
  { id: 'H', title: 'Social Welfare & Services', component: 'SectionH' },
  { id: 'I', title: 'Disaster Preparedness & Safety', component: 'SectionI' },
  { id: 'J', title: 'Cultural & Community Participation', component: 'SectionJ' },
  { id: 'K', title: 'Feedback & Aspirations', component: 'SectionK' }
]

const currentSectionIndex = computed(() => 
  sections.findIndex(s => s.id === currentSection.value)
)

const progressPercentage = computed(() => 
  ((currentSectionIndex.value + 1) / sections.length) * 100
)

const canGoNext = computed(() => 
  currentSectionIndex.value < sections.length - 1
)

const canGoPrevious = computed(() => 
  currentSectionIndex.value > 0
)

const goToNext = () => {
  if (canGoNext.value) {
    const nextIndex = currentSectionIndex.value + 1
    currentSection.value = sections[nextIndex].id
  }
}

const goToPrevious = () => {
  if (canGoPrevious.value) {
    const prevIndex = currentSectionIndex.value - 1
    currentSection.value = sections[prevIndex].id
  }
}

const handleSectionUpdate = (sectionId: string, data: any) => {
  formData.value[`section${sectionId}` as keyof typeof formData.value] = data
}

const handleSubmit = () => {
  emits('complete', {
    ...formData.value,
    userData: props.userData,
    completedAt: new Date().toISOString()
  })
}
</script>

<template>
  <div class="max-w-4xl mx-auto p-6">
    <!-- Header with Progress -->
    <Card class="mb-6">
      <CardHeader>
        <div class="flex items-center justify-between mb-4">
          <CardTitle class="text-xl">CFDP Digital Questionnaire</CardTitle>
          <div class="text-sm text-muted-foreground">
            Section {{ currentSection }} of {{ sections.length }}
          </div>
        </div>
        <Progress :model-value="progressPercentage" class="w-full" />
        <div class="text-sm text-muted-foreground mt-2">
          {{ Math.round(progressPercentage) }}% Complete
        </div>
      </CardHeader>
    </Card>

    <!-- Main Form Content -->
    <Tabs :model-value="currentSection" @update:model-value="currentSection = String($event)">
      <!-- Section Navigation Tabs -->
      <Card class="mb-6">
        <CardContent class="pt-6">
          <TabsList class="grid w-full grid-cols-11 gap-1">
            <TabsTrigger 
              v-for="section in sections" 
              :key="section.id"
              :value="section.id"
              class="text-xs"
            >
              {{ section.id }}
            </TabsTrigger>
          </TabsList>
        </CardContent>
      </Card>

      <!-- Section Content -->
      <TabsContent value="A">
        <SectionA 
          :user-data="userData"
          @update="(data) => handleSectionUpdate('A', data)"
        />
      </TabsContent>

      <TabsContent value="B">
        <SectionB 
          @update="(data) => handleSectionUpdate('B', data)"
        />
      </TabsContent>

      <TabsContent value="C">
        <SectionC 
          @update="(data) => handleSectionUpdate('C', data)"
        />
      </TabsContent>

      <!-- Placeholder for remaining sections -->
      <TabsContent 
        v-for="section in sections.slice(3)" 
        :key="section.id"
        :value="section.id"
      >
        <Card>
          <CardHeader>
            <CardTitle>Section {{ section.id }}: {{ section.title }}</CardTitle>
          </CardHeader>
          <CardContent>
            <p class="text-muted-foreground">This section will be implemented next.</p>
          </CardContent>
        </Card>
      </TabsContent>
    </Tabs>

    <!-- Navigation Controls -->
    <Card class="mt-6">
      <CardContent class="pt-6">
        <div class="flex justify-between">
          <Button 
            variant="outline" 
            @click="goToPrevious"
            :disabled="!canGoPrevious"
          >
            Previous
          </Button>
          
          <Button 
            v-if="canGoNext"
            @click="goToNext"
          >
            Next
          </Button>
          
          <Button 
            v-else
            @click="handleSubmit"
            class="bg-green-600 hover:bg-green-700"
          >
            Submit Survey
          </Button>
        </div>
      </CardContent>
    </Card>
  </div>
</template>