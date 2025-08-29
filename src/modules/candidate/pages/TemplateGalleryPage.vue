<template>
  <div class="grid grid-cols-1 lg:grid-cols-4 gap-6 p-4 sm:p-6 lg:p-8 bg-gray-50 min-h-screen font-sans rounded-lg shadow-md">
    <div class="lg:col-span-2">
      <div class="mb-6">
        <h1 class="text-3xl font-semibold text-gray-700 mb-2">{{ isEditing ? 'Edit Resume' : 'New Resume Application Form' }}</h1>
        <p class="text-gray-500 max-w-md">Please verify and update your information to complete the application.</p>
        <div class="mt-4 p-3 bg-blue-50 rounded-lg border border-blue-200">
          <p class="text-sm text-blue-700">
            <span class="font-medium">Selected Template:</span> {{ getTemplateName() }} 
            <button @click="changeTemplate" class="ml-2 text-blue-600 hover:text-blue-800 underline text-sm">
              Change Template
            </button>
          </p>
        </div>
      </div>

      <div class="mb-8 flex flex-wrap justify-center sm:justify-start gap-2">
        <div v-for="stepNum in totalSteps" :key="stepNum"
             @click="jumpToStep(stepNum)"
             :class="['flex items-center space-x-2 p-2 rounded-full cursor-pointer transition-all duration-300 transform hover:scale-105',
                      currentStep === stepNum ? 'bg-blue-600 text-white shadow-lg' : 'bg-gray-200 text-gray-700',
                      currentStep > stepNum ? 'bg-green-500 text-white' : ''
                     ]">
          <div :class="['w-8 h-8 flex items-center justify-center rounded-full font-bold',
                         currentStep === stepNum ? 'bg-white text-blue-600' : 'bg-gray-300 text-gray-700',
                         currentStep > stepNum ? 'bg-white text-green-500' : ''
                        ]">
            {{ stepNum }}
          </div>
          <span class="hidden lg:inline font-medium" :class="{'text-white': currentStep >= stepNum}">{{ getStepName(stepNum) }}</span>
        </div>
      </div>

      <div class="bg-white rounded-lg shadow p-6 space-y-6 mb-8">
        <transition name="fade" mode="out-in">
          <div :key="currentStep">
            <div v-if="currentStep === 1" class="space-y-6">
              <h2 class="text-xl font-semibold text-gray-700">Personal & Contact Information</h2>
              <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <div>
                  <label class="block text-sm font-medium text-gray-600">First Name <span class="text-red-500">*</span></label>
                  <input type="text" v-model="formData.personal.first_name" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Last Name <span class="text-red-500">*</span></label>
                  <input type="text" v-model="formData.personal.last_name" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Headline <span class="text-red-500">*</span></label>
                  <input type="text" v-model="formData.personal.headline" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required/>
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Country <span class="text-red-500">*</span></label>
                  <select v-model="formData.personal.country" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required>
                    <option value="">Select Country</option>
                    <option value="United States of America">United States of America</option>
                    <option value="Canada">Canada</option>
                    <option value="Mexico">Mexico</option>
                    <option value="India">India</option>
                    <option value="United Kingdom">United Kingdom</option>
                  </select>
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Address 1 <span class="text-red-500">*</span></label>
                  <input type="text" v-model="formData.personal.address_1" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Address 2</label>
                  <input type="text" v-model="formData.personal.address_2" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">City <span class="text-red-500">*</span></label>
                  <input type="text" v-model="formData.personal.city" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">State/Province/Region</label>
                  <input type="text" v-model="formData.personal.state_province_region" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Zip/Postal Code</label>
                  <input type="text" v-model="formData.personal.zip_postal_code" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Home Office</label>
                  <input type="text" v-model="formData.personal.home_office" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Geo Location</label>
                  <input type="text" v-model="formData.personal.geo_location" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">School District</label>
                  <input type="text" v-model="formData.personal.school_district" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Mobile Phone <span class="text-red-500">*</span></label>
                  <input type="text" v-model="formData.personal.mobile_phone" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Work Phone</label>
                  <input type="text" v-model="formData.personal.work_phone" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Home Phone</label>
                  <input type="text" v-model="formData.personal.home_phone" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Primary Email <span class="text-red-500">*</span></label>
                  <input type="email" v-model="formData.personal.email_0" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Secondary Email</label>
                  <input type="email" v-model="formData.personal.email_1" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">SSN</label>
                  <input type="text" v-model="formData.personal.ssn" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
                <div class="md:col-span-2">
                  <label class="block text-sm font-medium text-gray-600">Profile Photo <span class="text-red-500">*</span></label>
                  <input type="file" @change="e => handleFileChange(e, -1, 'personal', 'profile_photo_url')" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                  <img v-if="formData.personal.profile_photo_url" :src="formData.personal.profile_photo_url as string" class="mt-2 w-24 h-24 object-cover rounded-full" alt="Profile Photo Preview" />
                </div>
              </div>
            </div>

            <div v-if="currentStep === 2" class="space-y-6">
              <h2 class="text-xl font-semibold text-gray-700">Employment & Classification</h2>
              <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <div>
                  <label class="block text-sm font-medium text-gray-600">Employment Type <span class="text-red-500">*</span></label>
                  <select v-model="formData.employment.employment_type" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required>
                      <option value="">Select Employment Type</option>
                      <option value="Full-Time">Full-Time</option>
                      <option value="Part-Time">Part-Time</option>
                      <option value="Contract">Contract</option>
                  </select>
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Talent Status <span class="text-red-500">*</span></label>
                  <select v-model="formData.employment.talent_status" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required>
                    <option value="">Select Talent Status</option>
                    <option value="Active">Active</option>
                    <option value="Applicant">Applicant</option>
                    <option value="Deleted">Deleted</option>
                    <option value="DNA">DNA</option>
                    <option value="Inactive">Inactive</option>
                    <option value="Incomplete Applicant">Incomplete Applicant</option>
                    <option value="Online Applicant">Online Applicant</option>
                    <option value="Pending">Pending</option>
                    <option value="Terminated">Terminated</option>
                    <option value="Under Review">Under Review</option>
                  </select>
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Applicant Tags <span class="text-red-500">*</span></label>
                  <input type="text" v-model="formData.employment.applicant_tags" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Details / Notes</label>
                  <input type="text" v-model="formData.employment.details_notes" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Industry Experience</label>
                  <input type="text" v-model="formData.employment.industry_experience" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Applicant Source</label>
                  <input type="text" v-model="formData.employment.applicant_source" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
              </div>
            </div>

            <div v-if="currentStep === 3" class="space-y-6">
              <h2 class="text-xl font-semibold text-gray-700">Education History</h2>
              <div class="space-y-4 border border-gray-200 rounded-lg p-4 mt-6">
                <h3 class="text-lg font-medium text-gray-700 flex justify-between items-center">
                  Education History
                  <button type="button" @click="addRow('history', 'education_history')"
                          class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 transition-colors text-sm">
                    Add Education
                  </button>
                </h3>
                <div v-for="(edu, index) in formData.history.education_history" :key="index"
                     class="grid grid-cols-1 md:grid-cols-4 gap-4 p-4 border border-gray-200 rounded-md relative mb-4">
                  <div class="md:col-span-2">
                    <label class="block text-sm font-medium text-gray-600">Institution Name <span class="text-red-500">*</span></label>
                    <input type="text" v-model="edu.institution_name" @input="e => updateArrayField('history', 'education_history', index, 'institution_name', (e.target as HTMLInputElement).value)"
                           class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                  </div>
                  <div class="md:col-span-2">
                    <label class="block text-sm font-medium text-gray-600">Degree <span class="text-red-500">*</span></label>
                    <input type="text" v-model="edu.degree" @input="e => updateArrayField('history', 'education_history', index, 'degree', (e.target as HTMLInputElement).value)"
                           class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                  </div>
                  <div class="md:col-span-2">
                    <label class="block text-sm font-medium text-gray-600">Field of Study <span class="text-red-500">*</span></label>
                    <input type="text" v-model="edu.field_of_study" @input="e => updateArrayField('history', 'education_history', index, 'field_of_study', (e.target as HTMLInputElement).value)"
                           class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                  </div>
                  <div>
                    <label class="block text-sm font-medium text-gray-600">Education Location <span class="text-red-500">*</span></label>
                    <input type="text" v-model="edu.education_location" @input="e => updateArrayField('history', 'education_history', index, 'education_location', (e.target as HTMLInputElement).value)"
                           class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                  </div>
                  <div>
                    <label class="block text-sm font-medium text-gray-600">Start Date <span class="text-red-500">*</span></label>
                    <input type="date" v-model="edu.start_date" @input="e => updateArrayField('history', 'education_history', index, 'start_date', (e.target as HTMLInputElement).value)"
                           class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                  </div>
                  <div class="flex items-center space-x-2">
                    <input type="checkbox" v-model="edu.is_current_education" @change="e => {
                        updateArrayField('history', 'education_history', index, 'is_current_education', (e.target as HTMLInputElement).checked);
                        if ((e.target as HTMLInputElement).checked) {
                            updateArrayField('history', 'education_history', index, 'end_date', '');
                        }
                    }"
                           class="form-checkbox h-4 w-4 text-blue-600 rounded focus:ring-blue-500" />
                    <label class="text-sm font-medium text-gray-600">Currently Enrolled</label>
                  </div>
                  <div v-if="!edu.is_current_education">
                    <label class="block text-sm font-medium text-gray-600">End Date <span class="text-red-500">*</span></label>
                    <input type="date" v-model="edu.end_date" @input="e => updateArrayField('history', 'education_history', index, 'end_date', (e.target as HTMLInputElement).value)"
                           class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" :required="!edu.is_current_education" />
                  </div>
                  <div v-if="!edu.is_current_education" class="md:col-span-2">
                    <label class="block text-sm font-medium text-gray-600">Degree Image <span class="text-red-500">*</span></label>
                    <input type="file" @change="e => handleFileChange(e, index, 'education_history', 'degree_image_url')" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" :required="!edu.is_current_education" />
                  </div>
                  <button v-if="formData.history.education_history.length > 1" type="button" @click="removeRow('history', index, 'education_history')"
                          class="absolute top-2 right-2 text-red-500 hover:text-red-700 p-1 rounded-full">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                      <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
                    </svg>
                  </button>
                </div>
              </div>
            </div>

            <div v-if="currentStep === 4" class="space-y-6">
              <h2 class="text-xl font-semibold text-gray-700">Work History</h2>
              <div class="space-y-4 border border-gray-200 rounded-lg p-4">
                <h3 class="text-lg font-medium text-gray-700 flex justify-between items-center">
                  Work History
                  <button type="button" @click="addRow('history', 'work_history')"
                          class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 transition-colors text-sm">
                    Add Work
                  </button>
                </h3>
                <div v-for="(work, index) in formData.history.work_history" :key="index"
                     class="grid grid-cols-1 md:grid-cols-4 gap-4 p-4 border border-gray-200 rounded-md relative mb-4">
                  <div class="md:col-span-2">
                    <label class="block text-sm font-medium text-gray-600">Company Name <span class="text-red-500">*</span></label>
                    <input type="text" v-model="work.company_name" @input="e => updateArrayField('history', 'work_history', index, 'company_name', (e.target as HTMLInputElement).value)"
                           class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                  </div>
                  <div class="md:col-span-2">
                    <label class="block text-sm font-medium text-gray-600">Job Title <span class="text-red-500">*</span></label>
                    <input type="text" v-model="work.job_title" @input="e => updateArrayField('history', 'work_history', index, 'job_title', (e.target as HTMLInputElement).value)"
                           class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                  </div>
                  <div>
                    <label class="block text-sm font-medium text-gray-600">Job Type <span class="text-red-500">*</span></label>
                    <select v-model="work.job_type" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required>
                      <option value="">Select Job Type</option>
                      <option value="Full-Time">Full-Time</option>
                      <option value="Part-Time">Part-Time</option>
                      <option value="Contract">Contract</option>
                    </select>
                  </div>
                  <div>
                    <label class="block text-sm font-medium text-gray-600">Job Location <span class="text-red-500">*</span></label>
                    <input type="text" v-model="work.job_location" @input="e => updateArrayField('history', 'work_history', index, 'job_location', (e.target as HTMLInputElement).value)"
                           class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                  </div>
                  <div class="md:col-span-4">
                    <label class="block text-sm font-medium text-gray-600">Job Description</label>
                    <textarea v-model="work.job_description" @input="e => updateArrayField('history', 'work_history', index, 'job_description', (e.target as HTMLInputElement).value)"
                           class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm"></textarea>
                  </div>
                  <div>
                    <label class="block text-sm font-medium text-gray-600">Start Date <span class="text-red-500">*</span></label>
                    <input type="date" v-model="work.start_date" @input="e => updateArrayField('history', 'work_history', index, 'start_date', (e.target as HTMLInputElement).value)"
                           class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                  </div>
                  <div class="flex items-center space-x-2">
                    <input type="checkbox" v-model="work.is_current_job" @change="e => {
                        updateArrayField('history', 'work_history', index, 'is_current_job', (e.target as HTMLInputElement).checked);
                        if ((e.target as HTMLInputElement).checked) {
                            updateArrayField('history', 'work_history', index, 'end_date', '');
                        }
                    }"
                           class="form-checkbox h-4 w-4 text-blue-600 rounded focus:ring-blue-500" />
                    <label class="text-sm font-medium text-gray-600">Current Job</label>
                  </div>
                  <div v-if="!work.is_current_job">
                    <label class="block text-sm font-medium text-gray-600">End Date <span class="text-red-500">*</span></label>
                    <input type="date" v-model="work.end_date" @input="e => updateArrayField('history', 'work_history', index, 'end_date', (e.target as HTMLInputElement).value)"
                           class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" :required="!work.is_current_job" />
                  </div>
                  <div v-if="!work.is_current_job">
                      <label class="block text-sm font-medium text-gray-600">Experience Letter <span class="text-red-500">*</span></label>
                      <input type="file" @change="e => handleFileChange(e, index, 'work_history', 'experience_letter_url')" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" :required="!work.is_current_job" />
                  </div>
                  <button v-if="formData.history.work_history.length > 1" type="button" @click="removeRow('history', index, 'work_history')"
                          class="absolute top-2 right-2 text-red-500 hover:text-red-700 p-1 rounded-full">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                      <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
                    </svg>
                  </button>
                </div>
              </div>
            </div>

            <div v-if="currentStep === 5" class="space-y-6">
              <h2 class="text-xl font-semibold text-gray-700">Social Media & Version Control Profiles</h2>
              <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <div class="md:col-span-2">
                  <h3 class="text-lg font-medium text-gray-700 mb-2">Social Media Profiles</h3>
                  <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                      <div>
                        <label class="block text-sm font-medium text-gray-600">Facebook Profile</label>
                        <input type="text" v-model="formData.online_presence.facebook_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                      </div>
                      <div>
                        <label class="block text-sm font-medium text-gray-600">Twitter Profile</label>
                        <input type="text" v-model="formData.online_presence.twitter_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                      </div>
                      <div>
                        <label class="block text-sm font-medium text-gray-600">Instagram Profile</label>
                        <input type="text" v-model="formData.online_presence.instagram_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                      </div>
                      <div>
                        <label class="block text-sm font-medium text-gray-600">YouTube Profile</label>
                        <input type="text" v-model="formData.online_presence.youtube_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                      </div>
                      <div>
                        <label class="block text-sm font-medium text-gray-600">TikTok Profile</label>
                        <input type="text" v-model="formData.online_presence.tiktok_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                      </div>
                      <div>
                        <label class="block text-sm font-medium text-gray-600">Pinterest Profile</label>
                        <input type="text" v-model="formData.online_presence.pinterest_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                      </div>
                      <div>
                        <label class="block text-sm font-medium text-gray-600">Skype ID</label>
                        <input type="text" v-model="formData.online_presence.skype_id" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                      </div>
                      <div>
                        <label class="block text-sm font-medium text-gray-600">WhatsApp Number</label>
                        <input type="text" v-model="formData.online_presence.whatsapp_number" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                      </div>
                      <div>
                        <label class="block text-sm font-medium text-gray-600">WeChat ID</label>
                        <input type="text" v-model="formData.online_presence.wechat_id" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                      </div>
                      <div>
                        <label class="block text-sm font-medium text-gray-600">Viber ID</label>
                        <input type="text" v-model="formData.online_presence.viber_id" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                      </div>
                      <div>
                        <label class="block text-sm font-medium text-gray-600">Signal ID</label>
                        <input type="text" v-model="formData.online_presence.signal_id" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                      </div>
                      <div>
                        <label class="block text-sm font-medium text-gray-600">Telegram ID</label>
                        <input type="text" v-model="formData.online_presence.telegram_id" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                      </div>
                      <div>
                        <label class="block text-sm font-medium text-gray-600">Discord ID</label>
                        <input type="text" v-model="formData.online_presence.discord_id" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                      </div>
                      <div>
                        <label class="block text-sm font-medium text-gray-600">Slack ID</label>
                        <input type="text" v-model="formData.online_presence.slack_id" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                      </div>
                  </div>
                </div>

                <div class="md:col-span-2 mt-4">
                    <h3 class="text-lg font-medium text-gray-700 mb-2">Job Portal Profiles</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div>
                            <label class="block text-sm font-medium text-gray-600">LinkedIn Profile</label>
                            <input type="text" v-model="formData.job_portals.linked_in_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-600">Indeed Profile</label>
                            <input type="text" v-model="formData.job_portals.indeed_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-600">Monster Profile</label>
                            <input type="text" v-model="formData.job_portals.monster_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-600">Glassdoor Profile</label>
                            <input type="text" v-model="formData.job_portals.glassdoor_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-600">ZipRecruiter Profile</label>
                            <input type="text" v-model="formData.job_portals.zip_recruiter_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-600">CareerBuilder Profile</label>
                            <input type="text" v-model="formData.job_portals.career_builder_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-600">SimplyHired Profile</label>
                            <input type="text" v-model="formData.job_portals.simply_hired_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-600">Upwork Profile</label>
                            <input type="text" v-model="formData.job_portals.upwork_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-600">Freelancer Profile</label>
                            <input type="text" v-model="formData.job_portals.freelancer_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-600">Guru Profile</label>
                            <input type="text" v-model="formData.job_portals.guru_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-600">PeoplePerHour Profile</label>
                            <input type="text" v-model="formData.job_portals.people_per_hour_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-600">Fiverr Profile</label>
                            <input type="text" v-model="formData.job_portals.fiverr_profile" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                    </div>
                </div>

                <div class="md:col-span-2 mt-4">
                    <h3 class="text-lg font-medium text-gray-700 mb-2">Version Control Profiles</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div>
                            <label class="block text-sm font-medium text-gray-600">GitHub Profile</label>
                            <input type="text" v-model="formData.version_control.github" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-600">GitLab Profile</label>
                            <input type="text" v-model="formData.version_control.gitlab" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-600">Bitbucket Profile</label>
                            <input type="text" v-model="formData.version_control.bitbucket" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-600">SourceForge Profile</label>
                            <input type="text" v-model="formData.version_control.sourceforge" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-600">Codeberg Profile</label>
                            <input type="text" v-model="formData.version_control.codeberg" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-600">Gitea Profile</label>
                            <input type="text" v-model="formData.version_control.gitea" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                    </div>
                </div>
              </div>
            </div>

            <div v-if="currentStep === 6" class="space-y-6">
              <h2 class="text-xl font-semibold text-gray-700">Skills</h2>
              <div class="space-y-4 border border-gray-200 rounded-lg p-4 mt-6">
                <h3 class="text-lg font-medium text-gray-700 flex justify-between items-center">
                    Skills
                    <button type="button" @click="addRow('skills')"
                            class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 transition-colors text-sm">
                    Add Skill
                    </button>
                </h3>
                <div v-for="(skill, index) in formData.skills" :key="index"
                        class="grid grid-cols-1 md:grid-cols-4 gap-4 p-4 border border-gray-200 rounded-md relative mb-4">
                      <div>
                          <label class="block text-sm font-medium text-gray-600">Category <span class="text-red-500">*</span></label>
                          <input type="text" v-model="skill.skill_category_name" @input="e => updateArrayField('skills', undefined, index, 'skill_category_name', (e.target as HTMLInputElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                      </div>
                      <div>
                          <label class="block text-sm font-medium text-gray-600">Skill Name <span class="text-red-500">*</span></label>
                          <input type="text" v-model="skill.skill_name" @input="e => updateArrayField('skills', undefined, index, 'skill_name', (e.target as HTMLInputElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                      </div>
                      <div>
                          <label class="block text-sm font-medium text-gray-600">Proficiency Level <span class="text-red-500">*</span></label>
                          <input type="text" v-model="skill.skill_proficiency_level" @input="e => updateArrayField('skills', undefined, index, 'skill_proficiency_level', (e.target as HTMLInputElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                      </div>
                      <div>
                          <label class="block text-sm font-medium text-gray-600">Years of Experience</label>
                          <input type="number" v-model="skill.skill_years_of_experience" @input="e => updateArrayField('skills', undefined, index, 'skill_years_of_experience', (e.target as HTMLInputElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                      </div>
                      <div>
                          <label class="block text-sm font-medium text-gray-600">Notes</label>
                          <input type="text" v-model="skill.notes" @input="e => updateArrayField('skills', undefined, index, 'notes', (e.target as HTMLInputElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                      </div>
                      <button v-if="formData.skills.length > 1" type="button" @click="removeRow('skills', index)" class="absolute top-2 right-2 text-red-500 hover:text-red-700 p-1 rounded-full">
                          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                              <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
                          </svg>
                      </button>
                  </div>
              </div>
            </div>

            <div v-if="currentStep === 7" class="space-y-6">
                <h2 class="text-xl font-semibold text-gray-700">Certifications</h2>
                <div class="space-y-4 border border-gray-200 rounded-lg p-4 mt-6">
                <h3 class="text-lg font-medium text-gray-700 flex justify-between items-center">
                    Certifications
                    <button type="button" @click="addRow('certifications')"
                            class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 transition-colors text-sm">
                    Add Certification
                    </button>
                </h3>
                <div v-for="(cert, index) in formData.certifications" :key="index"
                        class="grid grid-cols-1 md:grid-cols-3 gap-4 p-4 border border-gray-200 rounded-md relative mb-4">
                    <div class="md:col-span-1">
                    <label class="block text-sm font-medium text-gray-600">Name <span class="text-red-500">*</span></label>
                    <input type="text" v-model="cert.certification_name" @input="e => updateArrayField('certifications', undefined, index, 'certification_name', (e.target as HTMLInputElement).value)"
                            class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                    </div>
                    <div class="md:col-span-1">
                    <label class="block text-sm font-medium text-gray-600">Body <span class="text-red-500">*</span></label>
                    <input type="text" v-model="cert.certification_body" @input="e => updateArrayField('certifications', undefined, index, 'certification_body', (e.target as HTMLInputElement).value)"
                            class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                    </div>
                    <div class="md:col-span-1">
                    <label class="block text-sm font-medium text-gray-600">Date <span class="text-red-500">*</span></label>
                    <input type="date" v-model="cert.certification_date" @input="e => updateArrayField('certifications', undefined, index, 'certification_date', (e.target as HTMLInputElement).value)"
                            class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                    </div>
                    <div class="md:col-span-1">
                      <label class="block text-sm font-medium text-gray-600">Status <span class="text-red-500">*</span></label>
                      <select v-model="cert.certification_status" @change="e => updateArrayField('certifications', undefined, index, 'certification_status', (e.target as HTMLSelectElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required>
                          <option value="Completed">Completed</option>
                          <option value="In Progress">In Progress</option>
                      </select>
                    </div>
                    <div v-if="cert.certification_status === 'Completed'" class="md:col-span-1">
                        <label class="block text-sm font-medium text-gray-600">Expiration Date <span class="text-red-500">*</span></label>
                        <input type="date" v-model="cert.expiration_date" @input="e => updateArrayField('certifications', undefined, index, 'expiration_date', (e.target as HTMLInputElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required/>
                    </div>
                    <div v-if="cert.certification_status === 'Completed'" class="md:col-span-2">
                        <label class="block text-sm font-medium text-gray-600">Certificate Picture <span class="text-red-500">*</span></label>
                        <input type="file" @change="e => handleFileChange(e, index, 'certifications', 'certificate_image_url')" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required/>
                    </div>
                    <button v-if="formData.certifications.length > 1" type="button" @click="removeRow('certifications', index)"
                            class="absolute top-2 right-2 text-red-500 hover:text-red-700 p-1 rounded-full">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                        <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
                    </svg>
                    </button>
                </div>
                </div>
            </div>

            <div v-if="currentStep === 8" class="space-y-6">
                <h2 class="text-xl font-semibold text-gray-700">Projects</h2>
                <div class="space-y-4 border border-gray-200 rounded-lg p-4 mt-6">
                  <h3 class="text-lg font-medium text-gray-700 flex justify-between items-center">
                    Projects
                    <button type="button" @click="addRow('projects')"
                            class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 transition-colors text-sm">
                    Add Project
                    </button>
                </h3>
                <div v-for="(project, index) in formData.projects" :key="index"
                        class="grid grid-cols-1 md:grid-cols-2 gap-4 p-4 border border-gray-200 rounded-md relative mb-4">
                        <div>
                          <label class="block text-sm font-medium text-gray-600">Project Name <span class="text-red-500">*</span></label>
                          <input type="text" v-model="project.proj_name" @input="e => updateArrayField('projects', undefined, index, 'proj_name', (e.target as HTMLInputElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                        </div>
                        <div>
                          <label class="block text-sm font-medium text-gray-600">Description <span class="text-red-500">*</span></label>
                          <input type="text" v-model="project.proj_description" @input="e => updateArrayField('projects', undefined, index, 'proj_description', (e.target as HTMLInputElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                        </div>
                        <div>
                          <label class="block text-sm font-medium text-gray-600">Date</label>
                          <input type="date" v-model="project.proj_date" @input="e => updateArrayField('projects', undefined, index, 'proj_date', (e.target as HTMLInputElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                          <label class="block text-sm font-medium text-gray-600">Relevant Link</label>
                          <input type="text" v-model="project.proj_relevantLink" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                        </div>
                        <div>
                          <label class="block text-sm font-medium text-gray-600">For Whom <span class="text-red-500">*</span></label>
                          <input type="text" v-model="project.proj_forWhom" @input="e => updateArrayField('projects', undefined, index, 'proj_forWhom', (e.target as HTMLInputElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required />
                        </div>
                        <button v-if="formData.projects.length > 1" type="button" @click="removeRow('projects', index)" class="absolute top-2 right-2 text-red-500 hover:text-red-700 p-1 rounded-full">
                          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                              <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
                          </svg>
                        </button>
                  </div>
                </div>
            </div>

            <div v-if="currentStep === 9" class="space-y-6">
                <h2 class="text-xl font-semibold text-gray-700">References</h2>
                <div class="mb-4">
                  <div v-if="!showReferences" class="text-center p-6 bg-gray-50 rounded-lg border-2 border-dashed border-gray-300">
                    <svg class="mx-auto h-12 w-12 text-gray-400 mb-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.196-2.196M17 20v-2a3 3 0 00-5.196-2.196M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.196-2.196M7 20v-2m0 0v-5a3 3 0 016 0v5m6 0V9a3 3 0 00-3-3H9a3 3 0 00-3 3v9.75" />
                    </svg>
                    <h3 class="text-lg font-medium text-gray-700 mb-2">Add Professional References</h3>
                    <p class="text-gray-500 mb-4">References are optional but can strengthen your application. Add contacts who can vouch for your professional experience.</p>
                    <button @click="enableReferences" type="button" class="px-6 py-3 bg-green-600 text-white rounded-lg hover:bg-green-700 transition-colors font-medium">
                      Add References
                    </button>
                  </div>
                  
                  <div v-if="showReferences" class="flex justify-between items-center mb-4">
                    <div>
                      <h3 class="text-lg font-medium text-gray-700">Professional References</h3>
                      <p class="text-sm text-gray-500">All reference fields are required once you choose to add references.</p>
                    </div>
                    <button @click="removeAllReferences" type="button" class="px-4 py-2 bg-red-100 text-red-700 rounded-lg hover:bg-red-200 transition-colors text-sm">
                      Remove All References
                    </button>
                  </div>
                </div>
                
                <div v-if="showReferences" class="space-y-4 border border-gray-200 rounded-lg p-4 mt-6">
                  <div class="flex justify-between items-center">
                    <h3 class="text-lg font-medium text-gray-700">References</h3>
                    <button type="button" @click="addRow('references')"
                            class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 transition-colors text-sm">
                      Add Another Reference
                    </button>
                  </div>
                  
                  <div v-for="(ref, index) in formData.references" :key="index"
                        class="grid grid-cols-1 md:grid-cols-2 gap-4 p-4 border border-gray-200 rounded-md relative mb-4">
                        <div>
                          <label class="block text-sm font-medium text-gray-600">Full Name <span class="text-red-500">*</span></label>
                          <input type="text" v-model="ref.full_name" @input="e => updateArrayField('references', undefined, index, 'full_name', (e.target as HTMLInputElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" :required="showReferences" />
                          <div v-if="showReferences && !ref.full_name.trim()" class="text-red-500 text-xs mt-1">Full name is required</div>
                        </div>
                        <div>
                          <label class="block text-sm font-medium text-gray-600">Email <span class="text-red-500">*</span></label>
                          <input type="email" v-model="ref.email" @input="e => updateArrayField('references', undefined, index, 'email', (e.target as HTMLInputElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" :required="showReferences" />
                          <div v-if="showReferences && !ref.email.trim()" class="text-red-500 text-xs mt-1">Email is required</div>
                        </div>
                        <div>
                          <label class="block text-sm font-medium text-gray-600">Phone Contact <span class="text-red-500">*</span></label>
                          <input type="text" v-model="ref.contact" @input="e => updateArrayField('references', undefined, index, 'contact', (e.target as HTMLInputElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" :required="showReferences" />
                          <div v-if="showReferences && !ref.contact.trim()" class="text-red-500 text-xs mt-1">Phone contact is required</div>
                        </div>
                        <div>
                          <label class="block text-sm font-medium text-gray-600">Job Title/Designation <span class="text-red-500">*</span></label>
                          <input type="text" v-model="ref.designation" @input="e => updateArrayField('references', undefined, index, 'designation', (e.target as HTMLInputElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" :required="showReferences" />
                          <div v-if="showReferences && !ref.designation.trim()" class="text-red-500 text-xs mt-1">Job title is required</div>
                        </div>
                        <div>
                          <label class="block text-sm font-medium text-gray-600">Company <span class="text-red-500">*</span></label>
                          <input type="text" v-model="ref.company" @input="e => updateArrayField('references', undefined, index, 'company', (e.target as HTMLInputElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" :required="showReferences" />
                          <div v-if="showReferences && !ref.company.trim()" class="text-red-500 text-xs mt-1">Company is required</div>
                        </div>
                        <div>
                          <label class="block text-sm font-medium text-gray-600">Professional Relationship <span class="text-red-500">*</span></label>
                          <select v-model="ref.relation" @change="e => updateArrayField('references', undefined, index, 'relation', (e.target as HTMLSelectElement).value)" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" :required="showReferences">
                            <option value="">Select relationship</option>
                            <option value="Direct Supervisor">Direct Supervisor</option>
                            <option value="Manager">Manager</option>
                            <option value="Colleague">Colleague</option>
                            <option value="Client">Client</option>
                            <option value="HR Representative">HR Representative</option>
                            <option value="Team Lead">Team Lead</option>
                            <option value="Project Manager">Project Manager</option>
                            <option value="Other">Other</option>
                          </select>
                          <div v-if="showReferences && !ref.relation.trim()" class="text-red-500 text-xs mt-1">Professional relationship is required</div>
                        </div>
                        <button v-if="formData.references.length > 1" type="button" @click="removeRow('references', index)" class="absolute top-2 right-2 text-red-500 hover:text-red-700 p-1 rounded-full bg-white shadow-sm">
                          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                              <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
                          </svg>
                        </button>
                  </div>
                  
                  <div class="mt-4 p-3 bg-blue-50 rounded-lg border border-blue-200">
                    <div class="flex items-start">
                      <svg class="h-5 w-5 text-blue-600 mt-0.5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                      </svg>
                      <div>
                        <h4 class="text-sm font-medium text-blue-800">Reference Guidelines</h4>
                        <p class="text-sm text-blue-700 mt-1">Choose references who can speak positively about your work performance, skills, and character. Always ask for permission before listing someone as a reference.</p>
                      </div>
                    </div>
                  </div>
                </div>
            </div>

            <div v-if="currentStep === 10" class="space-y-6">
              <h2 class="text-xl font-semibold text-gray-700">Additional Information</h2>
              <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <div class="md:col-span-2">
                  <label class="block text-sm font-medium text-gray-600">Resume Title <span class="text-red-500">*</span></label>
                  <input type="text" v-model="resumeTitle" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required placeholder="Enter a title for your resume" />
                </div>
                <div class="md:col-span-2">
                  <label class="block text-sm font-medium text-gray-600">Resume Text / Summary <span class="text-red-500">*</span></label>
                  <textarea v-model="formData.additional.resume_text" rows="6" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" required placeholder="Write a professional summary about yourself"></textarea>
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Driving License</label>
                  <input type="text" v-model="formData.personal.driving_license" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Passport</label>
                  <input type="text" v-model="formData.personal.passport" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Visa Status</label>
                  <input type="text" v-model="formData.personal.visa_status" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
                <div>
                  <label class="block text-sm font-medium text-gray-600">Security Clearance</label>
                  <input type="text" v-model="formData.personal.security_clearance" class="mt-1 p-2 border border-gray-300 rounded-md w-full focus:ring-blue-500 focus:border-blue-500 shadow-sm" />
                </div>
              </div>
            </div>
          </div>
        </transition>
      </div>

      <div class="flex justify-between">
        <button @click="currentStep--" :disabled="currentStep === 1"
                class="px-6 py-3 bg-gray-200 text-gray-700 rounded-md hover:bg-gray-300 transition-colors disabled:opacity-50 disabled:cursor-not-allowed">
          Previous
        </button>
        <button v-if="currentStep < totalSteps" @click="handleNextClick" :disabled="!canGoToNextStep"
                class="px-6 py-3 bg-blue-600 text-white rounded-md hover:bg-blue-700 transition-colors disabled:opacity-50 disabled:cursor-not-allowed">
          Next
        </button>
      </div>
    </div>

    <div class="lg:col-span-2">
      <div class="sticky top-4">
        <h2 class="text-2xl font-semibold text-gray-700 mb-4">Resume Preview</h2>
        <div class="bg-white rounded-lg shadow p-6">
          <div v-if="templateComponent">
            <component :is="templateComponent" :resume="formData" :palette="resumeStore.selectedPalette" />
          </div>
          <div v-else class="text-center text-gray-500 py-12">
            <p>Select a template from the gallery to see a preview.</p>
          </div>
        </div>
        <button @click="saveResume" :disabled="!canSaveResume" class="mt-4 w-full px-6 py-3 bg-green-600 text-white rounded-md hover:bg-green-700 transition-colors disabled:opacity-50 disabled:cursor-not-allowed">
          {{ isEditing ? 'Update Resume' : 'Save Resume' }}
        </button>
        <p v-if="saveMessage" :class="['mt-2 text-sm', saveMessage.includes('Error') ? 'text-red-600' : 'text-green-600']">
          {{ saveMessage }}
        </p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive, computed, onMounted, watch, unref } from 'vue' // ⬅️ replaced shallowRef with unref
import { useRouter, useRoute } from 'vue-router'
import { useResumeStore } from '@/stores/resumeStore'
import { useToastStore } from '@/stores/useToastStore'
import { ATS_TEMPLATES } from '@/constants/resumeTemplates'
import type { ResumeData } from '@/types/resume'
import type { Component } from 'vue'

const router = useRouter()
const route = useRoute()
const resumeStore = useResumeStore()
const toastStore = useToastStore()

const currentStep = ref(1)
const totalSteps = computed(() => 10)
const isEditing = computed(() => !!route.query.id)

const showReferences = ref(false)
const resumeTitle = ref('')
const saveMessage = ref('')
const isSaving = ref(false)

/* ✅ Return the actual component (not a Ref) */
const templateComponent = computed<Component | null>(() => {
  const template = ATS_TEMPLATES.find(t => t.id === resumeStore.selectedTemplate)
  return template ? (unref(template.layoutComponent) as Component) : null
})

const canSaveResume = computed(() => {
  const basicValidation = resumeTitle.value.trim() !== '' && 
         formData.additional.resume_text.trim() !== '' &&
         formData.personal.first_name.trim() !== '' &&
         formData.personal.last_name.trim() !== '' &&
         formData.personal.email_0.trim() !== ''
  
  if (showReferences.value) {
    const referencesValid = formData.references.every(ref => 
      ref.full_name.trim() !== '' &&
      ref.email.trim() !== '' &&
      (ref.contact?.trim() !== '' ?? false) &&
      ref.designation.trim() !== '' &&
      ref.company.trim() !== '' &&
      (ref.relation?.trim() !== '' ?? false)
    )
    return basicValidation && referencesValid
  }
  
  return basicValidation
})

const canGoToNextStep = computed(() => {
  switch (currentStep.value) {
    case 1:
      return formData.personal.first_name.trim() &&
             formData.personal.last_name.trim() &&
             formData.personal.headline.trim() &&
             formData.personal.country.trim() &&
             formData.personal.address_1.trim() &&
             formData.personal.city.trim() &&
             formData.personal.mobile_phone.trim() &&
             formData.personal.email_0.trim()
    case 2:
      return formData.employment.employment_type.trim() &&
             formData.employment.talent_status.trim() &&
             formData.employment.applicant_tags.trim()
    case 3:
      return formData.history.education_history.every(edu => 
        edu.institution_name.trim() &&
        edu.degree.trim() &&
        edu.field_of_study.trim() &&
        edu.education_location.trim() &&
        edu.start_date.trim() &&
        (edu.is_current_education || edu.end_date?.trim())
      )
    case 4:
      return formData.history.work_history.every(work =>
        work.company_name.trim() &&
        work.job_title.trim() &&
        work.job_type.trim() &&
        work.job_location.trim() &&
        work.start_date.trim() &&
        (work.is_current_job || work.end_date?.trim())
      )
    case 5:
      return true
    case 6:
      return formData.skills.every(skill =>
        skill.skill_category_name.trim() &&
        skill.skill_name.trim() &&
        skill.skill_proficiency_level.trim()
      )
    case 7:
        return formData.certifications.every(cert =>
        cert.certification_name.trim() &&
        cert.certification_body.trim() &&
        cert.certification_date.trim() &&
        (cert.certification_status !== 'Completed' || cert.expiration_date?.trim())
      )
    case 8:
      return formData.projects.every(project =>
        project.proj_name.trim() &&
        project.proj_description.trim() &&
        project.proj_forWhom.trim()
      )
    case 9:
      if (showReferences.value) {
        return formData.references.every(ref =>
          ref.full_name.trim() &&
          ref.email.trim() &&
          (ref.contact?.trim() ?? false) &&
          ref.designation.trim() &&
          ref.company.trim() &&
          (ref.relation?.trim() ?? false)
        )
      }
      return true
    case 10:
      return resumeTitle.value.trim() &&
             formData.additional.resume_text.trim()
    default:
      return true
  }
})

const formData = reactive<ResumeData>({
  personal: {
    first_name: '', last_name: '', headline: '', country: '', address_1: '', address_2: '', city: '', state_province_region: '', zip_postal_code: '', home_office: '', geo_location: '', school_district: '', mobile_phone: '', work_phone: '', home_phone: '', email_0: '', email_1: '', ssn: '', profile_photo_url: '', driving_license: '', passport: '', visa_status: '', security_clearance: ''
  },
  employment: {
    employment_type: '', talent_status: '', applicant_tags: '', details_notes: '', industry_experience: '', applicant_source: ''
  },
  history: {
    education_history: [{
      institution_name: '', degree: '', field_of_study: '', education_location: '', start_date: '', end_date: '', is_current_education: false, degree_image_url: ''
    }],
    work_history: [{
      company_name: '', job_title: '', job_type: '', job_location: '', job_description: '', start_date: '', end_date: '', is_current_job: false, experience_letter_url: ''
    }]
  },
  online_presence: {},
  job_portals: {},
  version_control: {},
  skills: [{
    skill_category_name: '', skill_name: '', skill_proficiency_level: '', skill_years_of_experience: 0, notes: ''
  }],
  certifications: [{
    certification_name: '', certification_body: '', certification_date: '', certification_status: 'Completed', expiration_date: '', certificate_image_url: ''
  }],
  projects: [{
    proj_name: '', proj_description: '', proj_date: '', proj_relevantLink: '', proj_forWhom: ''
  }],
  references: [{
    full_name: '', email: '', contact: '', designation: '', company: '', relation: ''
  }],
  additional: {
    resume_text: '', add_to_hotlist: false
  }
})

const handleNextClick = () => {
  if (canGoToNextStep.value) {
    currentStep.value++
  } else {
    // Show toast for validation error
  }
}

function enableReferences() {
  showReferences.value = true
  if (formData.references.length === 0) {
    formData.references.push({ full_name: '', email: '', contact: '', designation: '', company: '', relation: '' })
  }
}

function removeAllReferences() {
  showReferences.value = false
  formData.references = []
}

async function saveResume() {
  if (isSaving.value) return
  
  isSaving.value = true
  saveMessage.value = ''

  try {
    const validation = resumeStore.validateResumeData(formData, resumeTitle.value, showReferences.value)
    if (!validation.isValid) {
      saveMessage.value = `Error: ${validation.errors.join(', ')}`
      return
    }

    const resumeData = {
      id: isEditing.value ? route.query.id as string : undefined,
      title: resumeTitle.value.trim(),
      templateId: resumeStore.selectedTemplate,
      data: { 
        ...formData,
        references: showReferences.value ? formData.references : []
      }
    }

    const savedResume = await resumeStore.addOrUpdateResume(resumeData)
    
    if (savedResume) {
      saveMessage.value = isEditing.value ? 'Resume updated successfully!' : 'Resume saved successfully!'
      
      setTimeout(() => {
        router.push({ name: 'SavedResumes' })
      }, 1500)
    } else {
      throw new Error('Failed to save resume')
    }
  } catch (error) {
    console.error('Save error:', error)
    saveMessage.value = 'Error: Failed to save resume. Please try again.'
  } finally {
    isSaving.value = false
  }
}

onMounted(() => {
  if (isEditing.value && route.query.id) {
    const existingResume = resumeStore.savedResumes.find(r => r.id === route.query.id)
    if (existingResume) {
      Object.keys(formData).forEach(key => {
        const existingData = existingResume.data as any;
        if (existingData[key]) {
          const formDataKey = formData as any;
          if (typeof formDataKey[key] === 'object' && !Array.isArray(formDataKey[key])) {
            Object.assign(formDataKey[key], existingData[key])
          } else {
            formDataKey[key] = existingData[key]
          }
        }
      })
      
      resumeTitle.value = existingResume.title
      if (existingResume.data.references && existingResume.data.references.length > 0) {
        showReferences.value = true
      }
    }
  }
})

let saveTimeout: NodeJS.Timeout | null = null

const selectedTemplate = ref(resumeStore.selectedTemplate)

const getTemplateName = () => {
  const template = ATS_TEMPLATES.find(t => t.id === selectedTemplate.value)
  return template ? template.name : 'Default Template'
}

const changeTemplate = () => {
  router.push({ name: 'TemplateGallery' })
}

const getStepName = (step: number) => {
  switch (step) {
    case 1: return 'Personal Info'
    case 2: return 'Employment'
    case 3: return 'Education'
    case 4: return 'Work History'
    case 5: return 'Online Presence'
    case 6: return 'Skills'
    case 7: return 'Certifications'
    case 8: return 'Projects'
    case 9: return 'References'
    case 10: return 'Additional Info'
    default: return 'Step ' + step
  }
}

const jumpToStep = (stepNum: number) => {
  if (canGoToNextStep.value || stepNum < currentStep.value) {
    currentStep.value = stepNum
  } else {
    // Show toast for validation error
  }
}

const addRow = (section: keyof ResumeData, subsection?: keyof ResumeData['history']) => {
  if (section === 'history' && subsection === 'education_history') {
    formData.history.education_history.push({
      institution_name: '', degree: '', field_of_study: '', education_location: '', start_date: '', end_date: '', is_current_education: false, degree_image_url: ''
    })
  } else if (section === 'history' && subsection === 'work_history') {
    formData.history.work_history.push({
      company_name: '', job_title: '', job_type: '', job_location: '', job_description: '', start_date: '', end_date: '', is_current_job: false, experience_letter_url: ''
    })
  } else if (section === 'skills') {
    formData.skills.push({
      skill_category_name: '', skill_name: '', skill_proficiency_level: '', skill_years_of_experience: 0, notes: ''
    })
  } else if (section === 'certifications') {
    formData.certifications.push({
      certification_name: '', certification_body: '', certification_date: '', certification_status: 'Completed', expiration_date: '', certificate_image_url: ''
    })
  } else if (section === 'projects') {
    formData.projects.push({
      proj_name: '', proj_description: '', proj_date: '', proj_relevantLink: '', proj_forWhom: ''
    })
  } else if (section === 'references') {
    formData.references.push({
      full_name: '', email: '', contact: '', designation: '', company: '', relation: ''
    })
  }
}

const removeRow = (section: keyof ResumeData, index: number, subsection?: keyof ResumeData['history']) => {
  if (section === 'history' && subsection === 'education_history') {
    formData.history.education_history.splice(index, 1)
  } else if (section === 'history' && subsection === 'work_history') {
    formData.history.work_history.splice(index, 1)
  } else if (section === 'skills') {
    formData.skills.splice(index, 1)
  } else if (section === 'certifications') {
    formData.certifications.splice(index, 1)
  } else if (section === 'projects') {
    formData.projects.splice(index, 1)
  } else if (section === 'references') {
    formData.references.splice(index, 1)
  }
}

const updateArrayField = (section: keyof ResumeData, subsection: keyof ResumeData['history'] | undefined, index: number, field: string, value: any) => {
  if (section === 'history' && subsection === 'education_history') {
    (formData.history.education_history[index] as any)[field] = value
  } else if (section === 'history' && subsection === 'work_history') {
    (formData.history.work_history[index] as any)[field] = value
  } else if (section === 'skills') {
    (formData.skills[index] as any)[field] = value
  } else if (section === 'certifications') {
    (formData.certifications[index] as any)[field] = value
  } else if (section === 'projects') {
    (formData.projects[index] as any)[field] = value
  } else if (section === 'references') {
    (formData.references[index] as any)[field] = value
  }
}

const handleFileChange = async (event: Event, index: number, section: string, field: string) => {
  const target = event.target as HTMLInputElement
  if (target.files && target.files.length > 0) {
    const file = target.files[0]
    
    const maxSize = 5 * 1024 * 1024 // 5MB
    if (file.size > maxSize) {
      saveMessage.value = 'Error: File size must be less than 5MB'
      return
    }
    
    try {
      const base64 = await convertToBase64(file)
      if (section === 'personal' && field === 'profile_photo_url') {
        formData.personal.profile_photo_url = base64 as string
      } else if (section === 'history' && field === 'degree_image_url') {
        (formData.history.education_history[index] as any)[field] = base64;
      } else if (section === 'history' && field === 'experience_letter_url') {
        (formData.history.work_history[index] as any)[field] = base64;
      } else if (section === 'certifications') {
        (formData.certifications[index] as any)[field] = base64;
      }
    } catch (error) {
      console.error('File upload error:', error)
      saveMessage.value = 'Error: Failed to upload file'
    }
  }
}

const convertToBase64 = (file: File): Promise<string | ArrayBuffer | null> => {
  return new Promise((resolve, reject) => {
    const reader = new FileReader()
    reader.onload = () => resolve(reader.result)
    reader.onerror = error => reject(error)
    reader.readAsDataURL(file)
  })
}

watch(formData, () => {
  if (saveTimeout) {
    clearTimeout(saveTimeout)
  }
  saveTimeout = setTimeout(() => {
    resumeStore.updateCurrentResume(formData)
  }, 1000)
}, { deep: true })
</script>


<style scoped>
/* Enhanced styles for better UX */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.form-checkbox:checked {
  background-color: #3b82f6;
  border-color: #3b82f6;
}

.form-checkbox:focus {
  ring-color: #3b82f6;
  ring-opacity: 0.5;
}

/* Smooth transitions for step navigation */
.step-indicator {
  transition: all 0.3s ease;
}

/* Better file input styling */
input[type="file"] {
  transition: border-color 0.2s ease;
}

input[type="file"]:hover {
  border-color: #3b82f6;
}

/* Enhanced button hover effects */
button:not(:disabled):hover {
  transform: translateY(-1px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

button:not(:disabled):active {
  transform: translateY(0);
}

/* Loading state for save button */
.save-button:disabled {
  position: relative;
  color: transparent;
}

.save-button:disabled::after {
  content: '';
  position: absolute;
  width: 16px;
  height: 16px;
  top: 50%;
  left: 50%;
  margin-left: -8px;
  margin-top: -8px;
  border: 2px solid #ffffff;
  border-radius: 50%;
  border-top-color: transparent;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

/* Better error message styling */
.error-message {
  animation: shake 0.5s ease-in-out;
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-5px); }
  75% { transform: translateX(5px); }
}

/* Improved form field focus states */
input:focus,
select:focus,
textarea:focus {
  outline: none;
  border-color: #3b82f6;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
}

/* Better spacing for mobile */
@media (max-width: 768px) {
  .grid {
    gap: 1rem;
  }
  
  .p-4 {
    padding: 1rem;
  }
}
</style>