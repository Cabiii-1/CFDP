<script setup lang="ts">
import { ref, watch, computed } from 'vue'
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import { Button } from '@/components/ui/button'
import { Input } from '@/components/ui/input'
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from '@/components/ui/select'
import { Table, TableBody, TableCell, TableHead, TableHeader, TableRow } from '@/components/ui/table'
import { Badge } from '@/components/ui/badge'
import { PlusIcon, XIcon } from 'lucide-vue-next'

const emits = defineEmits<{
  update: [data: any]
}>()

interface HouseholdMember {
  id: string
  fullName: string
  relationship: string
  age: string
  sex: string
  civilStatus: string
  education: string
  occupation: string
  income: string
  philHealth: string
  fourPs: string
  pcicInsurance: string
}

const householdMembers = ref<HouseholdMember[]>([])

const formData = computed(() => ({
  householdMembers: householdMembers.value,
  totalMembers: householdMembers.value.length
}))

watch(formData, (newData) => {
  emits('update', newData)
}, { deep: true })

const relationshipOptions = [
  'Spouse', 'Child', 'Parent', 'Sibling', 'Grandchild', 
  'Grandparent', 'In-law', 'Other relative', 'Non-relative'
]

const civilStatusOptions = [
  'Single', 'Married', 'Widowed', 'Separated', 'Divorced'
]

const addMember = () => {
  const newMember: HouseholdMember = {
    id: Date.now().toString(),
    fullName: '',
    relationship: '',
    age: '',
    sex: '',
    civilStatus: '',
    education: '',
    occupation: '',
    income: '',
    philHealth: '',
    fourPs: '',
    pcicInsurance: ''
  }
  householdMembers.value.push(newMember)
}

const removeMember = (id: string) => {
  const index = householdMembers.value.findIndex(member => member.id === id)
  if (index > -1) {
    householdMembers.value.splice(index, 1)
  }
}

// Add one member by default
if (householdMembers.value.length === 0) {
  addMember()
}
</script>

<template>
  <Card>
    <CardHeader>
      <div class="flex items-center justify-between">
        <CardTitle>Section C: Household Composition</CardTitle>
        <div class="flex items-center gap-2">
          <Badge variant="secondary">
            Total Members: {{ householdMembers.length }}
          </Badge>
        </div>
      </div>
    </CardHeader>
    <CardContent class="space-y-6">
      <!-- Add Member Button -->
      <div class="flex justify-end">
        <Button @click="addMember" class="flex items-center gap-2">
          <PlusIcon class="h-4 w-4" />
          Add Household Member
        </Button>
      </div>

      <!-- Members Table -->
      <div v-if="householdMembers.length > 0" class="overflow-x-auto">
        <Table>
          <TableHeader>
            <TableRow>
              <TableHead class="min-w-[200px]">Full Name</TableHead>
              <TableHead class="min-w-[150px]">Relationship</TableHead>
              <TableHead class="min-w-[80px]">Age</TableHead>
              <TableHead class="min-w-[100px]">Sex</TableHead>
              <TableHead class="min-w-[120px]">Civil Status</TableHead>
              <TableHead class="min-w-[150px]">Education</TableHead>
              <TableHead class="min-w-[150px]">Occupation</TableHead>
              <TableHead class="min-w-[120px]">Income (PHP)</TableHead>
              <TableHead class="min-w-[100px]">PhilHealth</TableHead>
              <TableHead class="min-w-[80px]">4Ps</TableHead>
              <TableHead class="min-w-[80px]">PCIC</TableHead>
              <TableHead class="w-[80px]">Action</TableHead>
            </TableRow>
          </TableHeader>
          <TableBody>
            <TableRow v-for="member in householdMembers" :key="member.id">
              <!-- Full Name -->
              <TableCell>
                <Input
                  v-model="member.fullName"
                  placeholder="Full name"
                  class="min-w-[180px]"
                />
              </TableCell>

              <!-- Relationship -->
              <TableCell>
                <Select v-model="member.relationship">
                  <SelectTrigger class="min-w-[130px]">
                    <SelectValue placeholder="Select" />
                  </SelectTrigger>
                  <SelectContent>
                    <SelectItem 
                      v-for="relationship in relationshipOptions" 
                      :key="relationship" 
                      :value="relationship"
                    >
                      {{ relationship }}
                    </SelectItem>
                  </SelectContent>
                </Select>
              </TableCell>

              <!-- Age -->
              <TableCell>
                <Input
                  v-model="member.age"
                  type="number"
                  min="1"
                  max="150"
                  placeholder="Age"
                  class="w-[70px]"
                />
              </TableCell>

              <!-- Sex -->
              <TableCell>
                <Select v-model="member.sex">
                  <SelectTrigger class="w-[90px]">
                    <SelectValue placeholder="Sex" />
                  </SelectTrigger>
                  <SelectContent>
                    <SelectItem value="Male">Male</SelectItem>
                    <SelectItem value="Female">Female</SelectItem>
                  </SelectContent>
                </Select>
              </TableCell>

              <!-- Civil Status -->
              <TableCell>
                <Select v-model="member.civilStatus">
                  <SelectTrigger class="min-w-[110px]">
                    <SelectValue placeholder="Status" />
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
              </TableCell>

              <!-- Education -->
              <TableCell>
                <Input
                  v-model="member.education"
                  placeholder="Education"
                  class="min-w-[130px]"
                />
              </TableCell>

              <!-- Occupation -->
              <TableCell>
                <Input
                  v-model="member.occupation"
                  placeholder="Occupation"
                  class="min-w-[130px]"
                />
              </TableCell>

              <!-- Income -->
              <TableCell>
                <Input
                  v-model="member.income"
                  type="number"
                  min="0"
                  step="0.01"
                  placeholder="0.00"
                  class="min-w-[100px]"
                />
              </TableCell>

              <!-- PhilHealth -->
              <TableCell>
                <Select v-model="member.philHealth">
                  <SelectTrigger class="w-[90px]">
                    <SelectValue placeholder="PH" />
                  </SelectTrigger>
                  <SelectContent>
                    <SelectItem value="Yes">Yes</SelectItem>
                    <SelectItem value="No">No</SelectItem>
                  </SelectContent>
                </Select>
              </TableCell>

              <!-- 4Ps -->
              <TableCell>
                <Select v-model="member.fourPs">
                  <SelectTrigger class="w-[70px]">
                    <SelectValue placeholder="4Ps" />
                  </SelectTrigger>
                  <SelectContent>
                    <SelectItem value="Yes">Yes</SelectItem>
                    <SelectItem value="No">No</SelectItem>
                  </SelectContent>
                </Select>
              </TableCell>

              <!-- PCIC -->
              <TableCell>
                <Select v-model="member.pcicInsurance">
                  <SelectTrigger class="w-[70px]">
                    <SelectValue placeholder="PCIC" />
                  </SelectTrigger>
                  <SelectContent>
                    <SelectItem value="Yes">Yes</SelectItem>
                    <SelectItem value="No">No</SelectItem>
                  </SelectContent>
                </Select>
              </TableCell>

              <!-- Remove Button -->
              <TableCell>
                <Button
                  variant="destructive"
                  size="sm"
                  @click="removeMember(member.id)"
                  class="h-8 w-8 p-0"
                >
                  <XIcon class="h-4 w-4" />
                </Button>
              </TableCell>
            </TableRow>
          </TableBody>
        </Table>
      </div>

      <!-- Empty State -->
      <div v-else class="text-center py-8 text-muted-foreground">
        <p>No household members added yet.</p>
        <Button @click="addMember" class="mt-4">
          Add First Member
        </Button>
      </div>
    </CardContent>
  </Card>
</template>