<script setup lang="ts">
import { ref, watch } from 'vue'
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import { Input } from '@/components/ui/input'
import { Label } from '@/components/ui/label'
import { RadioGroup, RadioGroupItem } from '@/components/ui/radio-group'
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from '@/components/ui/select'

interface UserData {
  enumeratorName: string
  supervisorName: string
}

const props = defineProps<{
  userData: UserData
}>()

const emits = defineEmits<{
  update: [data: any]
}>()

const formData = ref({
  householdId: '',
  barangay: '',
  municipality: '',
  legislativeDistrict: '',
  enumeratorName: props.userData.enumeratorName,
  dateOfInterview: new Date().toISOString().split('T')[0]
})

watch(formData, (newData) => {
  emits('update', newData)
}, { deep: true })

const barangays = [
  'Barangay 1', 'Barangay 2', 'Barangay 3', 'Barangay 4', 'Barangay 5'
]

const municipalities = [
  'Municipality A', 'Municipality B', 'Municipality C', 'Municipality D'
]
</script>

<template>
  <Card>
    <CardHeader>
      <CardTitle>Section A: Household Identification</CardTitle>
    </CardHeader>
    <CardContent class="grid gap-6">
      <!-- Household ID -->
      <div class="grid gap-3">
        <Label for="household-id">Household ID *</Label>
        <Input
          id="household-id"
          v-model="formData.householdId"
          placeholder="Enter unique household identifier"
          required
        />
      </div>

      <!-- Barangay -->
      <div class="grid gap-3">
        <Label for="barangay">Barangay *</Label>
        <Select v-model="formData.barangay">
          <SelectTrigger>
            <SelectValue placeholder="Select barangay" />
          </SelectTrigger>
          <SelectContent>
            <SelectItem 
              v-for="barangay in barangays" 
              :key="barangay" 
              :value="barangay"
            >
              {{ barangay }}
            </SelectItem>
          </SelectContent>
        </Select>
      </div>

      <!-- Municipality -->
      <div class="grid gap-3">
        <Label for="municipality">Municipality *</Label>
        <Select v-model="formData.municipality">
          <SelectTrigger>
            <SelectValue placeholder="Select municipality" />
          </SelectTrigger>
          <SelectContent>
            <SelectItem 
              v-for="municipality in municipalities" 
              :key="municipality" 
              :value="municipality"
            >
              {{ municipality }}
            </SelectItem>
          </SelectContent>
        </Select>
      </div>

      <!-- Legislative District -->
      <div class="grid gap-3">
        <Label>Legislative District *</Label>
        <RadioGroup v-model="formData.legislativeDistrict">
          <div class="flex items-center space-x-2">
            <RadioGroupItem value="1" id="district-1" />
            <Label for="district-1">District 1</Label>
          </div>
          <div class="flex items-center space-x-2">
            <RadioGroupItem value="2" id="district-2" />
            <Label for="district-2">District 2</Label>
          </div>
          <div class="flex items-center space-x-2">
            <RadioGroupItem value="3" id="district-3" />
            <Label for="district-3">District 3</Label>
          </div>
        </RadioGroup>
      </div>

      <!-- Enumerator Name (Pre-filled, Disabled) -->
      <div class="grid gap-3">
        <Label for="enumerator-name">Enumerator Name</Label>
        <Input
          id="enumerator-name"
          v-model="formData.enumeratorName"
          disabled
          class="bg-muted"
        />
      </div>

      <!-- Date of Interview -->
      <div class="grid gap-3">
        <Label for="date-interview">Date of Interview *</Label>
        <Input
          id="date-interview"
          v-model="formData.dateOfInterview"
          type="date"
          required
        />
      </div>
    </CardContent>
  </Card>
</template>