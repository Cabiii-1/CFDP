<script setup lang="ts">
import { ref, watch } from 'vue'
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import { Input } from '@/components/ui/input'
import { Label } from '@/components/ui/label'
import { RadioGroup, RadioGroupItem } from '@/components/ui/radio-group'
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from '@/components/ui/select'
import { Textarea } from '@/components/ui/textarea'

const emits = defineEmits<{
  update: [data: any]
}>()

const formData = ref({
  firstName: '',
  middleName: '',
  lastName: '',
  sex: '',
  age: '',
  civilStatus: '',
  ethnicity: '',
  religion: '',
  education: '',
  occupation: '',
  monthlyIncome: '',
  philHealth: '',
  fourPs: '',
  pcicInsurance: '',
  otherPrograms: '',
  otherProgramsDetails: ''
})

watch(formData, (newData) => {
  emits('update', newData)
}, { deep: true })

const civilStatusOptions = [
  'Single', 'Married', 'Widowed', 'Separated', 'Divorced'
]

const educationLevels = [
  'No formal education',
  'Elementary (some)',
  'Elementary graduate',
  'High School (some)',
  'High School graduate',
  'College (some)',
  'College graduate',
  'Post-graduate'
]
</script>

<template>
  <Card>
    <CardHeader>
      <CardTitle>Section B: Head of Family Profile</CardTitle>
    </CardHeader>
    <CardContent class="grid gap-6">
      <!-- Name Fields -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
        <div class="grid gap-3">
          <Label for="first-name">First Name *</Label>
          <Input
            id="first-name"
            v-model="formData.firstName"
            placeholder="First name"
            required
          />
        </div>
        <div class="grid gap-3">
          <Label for="middle-name">Middle Name</Label>
          <Input
            id="middle-name"
            v-model="formData.middleName"
            placeholder="Middle name"
          />
        </div>
        <div class="grid gap-3">
          <Label for="last-name">Last Name *</Label>
          <Input
            id="last-name"
            v-model="formData.lastName"
            placeholder="Last name"
            required
          />
        </div>
      </div>

      <!-- Sex -->
      <div class="grid gap-3">
        <Label>Sex *</Label>
        <RadioGroup v-model="formData.sex">
          <div class="flex items-center space-x-2">
            <RadioGroupItem value="Male" id="male" />
            <Label for="male">Male</Label>
          </div>
          <div class="flex items-center space-x-2">
            <RadioGroupItem value="Female" id="female" />
            <Label for="female">Female</Label>
          </div>
        </RadioGroup>
      </div>

      <!-- Age -->
      <div class="grid gap-3">
        <Label for="age">Age *</Label>
        <Input
          id="age"
          v-model="formData.age"
          type="number"
          min="1"
          max="150"
          placeholder="Age in years"
          required
        />
      </div>

      <!-- Civil Status -->
      <div class="grid gap-3">
        <Label for="civil-status">Civil Status *</Label>
        <Select v-model="formData.civilStatus">
          <SelectTrigger>
            <SelectValue placeholder="Select civil status" />
          </SelectTrigger>
          <SelectContent>
            <SelectItem 
              v-for="status in civilStatusOptions" 
              :key="status" 
              :value="status"
            >
              {{ status }}
            </SelectItem>
          </SelectContent>
        </Select>
      </div>

      <!-- Ethnicity -->
      <div class="grid gap-3">
        <Label for="ethnicity">Ethnicity</Label>
        <Input
          id="ethnicity"
          v-model="formData.ethnicity"
          placeholder="e.g., Tagalog, Ilocano, Cebuano, etc."
        />
      </div>

      <!-- Religion -->
      <div class="grid gap-3">
        <Label for="religion">Religion</Label>
        <Input
          id="religion"
          v-model="formData.religion"
          placeholder="e.g., Roman Catholic, Protestant, Islam, etc."
        />
      </div>

      <!-- Education -->
      <div class="grid gap-3">
        <Label for="education">Highest Educational Attainment</Label>
        <Select v-model="formData.education">
          <SelectTrigger>
            <SelectValue placeholder="Select education level" />
          </SelectTrigger>
          <SelectContent>
            <SelectItem 
              v-for="level in educationLevels" 
              :key="level" 
              :value="level"
            >
              {{ level }}
            </SelectItem>
          </SelectContent>
        </Select>
      </div>

      <!-- Occupation -->
      <div class="grid gap-3">
        <Label for="occupation">Occupation/Income Source</Label>
        <Input
          id="occupation"
          v-model="formData.occupation"
          placeholder="e.g., Farmer, Teacher, Self-employed, etc."
        />
      </div>

      <!-- Monthly Income -->
      <div class="grid gap-3">
        <Label for="monthly-income">Monthly Income (PHP)</Label>
        <Input
          id="monthly-income"
          v-model="formData.monthlyIncome"
          type="number"
          min="0"
          step="0.01"
          placeholder="0.00"
        />
      </div>

      <!-- Program Memberships -->
      <div class="space-y-6">
        <h3 class="text-lg font-medium">Program Memberships</h3>
        
        <!-- PhilHealth -->
        <div class="grid gap-3">
          <Label>PhilHealth Member</Label>
          <RadioGroup v-model="formData.philHealth">
            <div class="flex items-center space-x-2">
              <RadioGroupItem value="Yes" id="philhealth-yes" />
              <Label for="philhealth-yes">Yes</Label>
            </div>
            <div class="flex items-center space-x-2">
              <RadioGroupItem value="No" id="philhealth-no" />
              <Label for="philhealth-no">No</Label>
            </div>
          </RadioGroup>
        </div>

        <!-- 4Ps -->
        <div class="grid gap-3">
          <Label>4Ps (Pantawid Pamilyang Pilipino Program) Beneficiary</Label>
          <RadioGroup v-model="formData.fourPs">
            <div class="flex items-center space-x-2">
              <RadioGroupItem value="Yes" id="4ps-yes" />
              <Label for="4ps-yes">Yes</Label>
            </div>
            <div class="flex items-center space-x-2">
              <RadioGroupItem value="No" id="4ps-no" />
              <Label for="4ps-no">No</Label>
            </div>
          </RadioGroup>
        </div>

        <!-- PCIC Insurance -->
        <div class="grid gap-3">
          <Label>PCIC (Philippine Crop Insurance Corporation) Member</Label>
          <RadioGroup v-model="formData.pcicInsurance">
            <div class="flex items-center space-x-2">
              <RadioGroupItem value="Yes" id="pcic-yes" />
              <Label for="pcic-yes">Yes</Label>
            </div>
            <div class="flex items-center space-x-2">
              <RadioGroupItem value="No" id="pcic-no" />
              <Label for="pcic-no">No</Label>
            </div>
          </RadioGroup>
        </div>

        <!-- Other Programs -->
        <div class="grid gap-3">
          <Label>Member of Other Programs</Label>
          <RadioGroup v-model="formData.otherPrograms">
            <div class="flex items-center space-x-2">
              <RadioGroupItem value="Yes" id="other-yes" />
              <Label for="other-yes">Yes</Label>
            </div>
            <div class="flex items-center space-x-2">
              <RadioGroupItem value="No" id="other-no" />
              <Label for="other-no">No</Label>
            </div>
          </RadioGroup>
        </div>

        <!-- Other Programs Details (Conditional) -->
        <div v-if="formData.otherPrograms === 'Yes'" class="grid gap-3">
          <Label for="other-programs-details">Please specify other programs</Label>
          <Textarea
            id="other-programs-details"
            v-model="formData.otherProgramsDetails"
            placeholder="List other programs you are a member of..."
            rows="3"
          />
        </div>
      </div>
    </CardContent>
  </Card>
</template>