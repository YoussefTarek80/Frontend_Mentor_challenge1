<template>
    <section class="text-white flex items-center justify-center flex-col gap-5 p-9">
        <div class="header">
            <div class="logo"><img src="../assets/images/logo-full.svg" alt="Logo"></div>
        </div>
        <div class="description flex items-center flex-col gap-2 justify-center text-center">
            <div class="sm:w-8/12 sm:text-5xl text-2xl font-bold">
                <h1>Your Journey to Coding Conf 2025 Starts Here!</h1>
            </div>
            <div class="text-2xl text-gray-400">
                <span>Secure your spot at next year's biggest coding conference</span>
            </div>
        </div>

        <form 
            @submit.prevent="generate" 
            class="flex flex-col items-center justify-center sm:w-3/4 w-full max-w-lg space-y-6 my-2 z-50"
        >
            <!-- Upload Avatar -->
            <div class="flex flex-col w-full">
                <label class="mb-2 text-neutral-200">Upload Avatar</label>
                <div 
                    class="relative flex flex-col items-center bg-neutral-700 justify-center border-2 border-dashed border-neutral-400 rounded-lg p-6 w-full text-center cursor-pointer transition-all duration-300"
                    @click="triggerFileUpload"
                    @keydown.enter="triggerFileUpload"
                    tabindex="0"
                >
                    <img src="../assets/images/icon-upload.svg" v-if="!imagePreview" class="p-1 absolute top-6 bg-neutral-600 rounded-xl" alt="Upload Icon">
                    
                    <p v-if="!imagePreview" class="mt-14 text-gray-600 text-sm">Drag and drop or click to upload</p>

                    <img v-if="imagePreview" :src="imagePreview" class="w-24 h-24 rounded-full object-cover" alt="Uploaded Avatar">
                </div>

                <input 
                    ref="fileInput"
                    type="file"
                    class="hidden"
                    accept="image/png, image/jpeg"
                    @change="handleFileChange"
                />

                <div class="flex items-center gap-2 text-neutral-500 my-2">
                    <img src="../assets/images/icon-info.svg" alt="Info">
                    <span>Upload your photo (JPG or PNG, max size: 500KB)</span>
                </div>
                <div v-if="fileError" class="text-red-500 text-sm">{{ fileError }}</div>
            </div>

            <div class="flex flex-col w-full">
                <label for="fullName" class="text-neutral-200">Full Name</label>
                <input v-model="fullName" type="text" id="fullName" class="normal-input w-full" aria-label="Full Name">
                <div v-if="nameError" class="text-red-500 text-sm">{{ nameError }}</div>
            </div>

            <div class="flex flex-col w-full">
                <label for="email" class="text-neutral-200">Email Address</label>
                <input v-model="email" type="text" id="email" class="normal-input w-full" aria-label="Email Address" placeholder="example@email.com">
                <div v-if="emailError" class="text-red-500 text-sm">{{ emailError }}</div>
            </div>

            <div class="flex flex-col w-full">
                <label for="github" class="text-neutral-200">GitHub Username</label>
                <input 
                    v-model="github" 
                    type="text" 
                    id="github" 
                    class="normal-input w-full" 
                    aria-label="GitHub Username" 
                    placeholder="@yourusername"
                    @keydown.enter="generate"
                >
                <div v-if="githubError" class="text-red-500 text-sm">{{ githubError }}</div>
            </div>

            <button 
                type="submit" 
                class="text-neutral-700 bg-orange-500 font-extrabold py-3 px-6 rounded-lg w-full hover:bg-orange-600 transition-all duration-300"
                @keydown.enter="generate"
            >
                Generate My Ticket
            </button>
        </form>
    </section>
</template>

<script setup>
import { ref, inject } from 'vue';

const data = inject('data');

const fullName = ref("");
const email = ref("");
const github = ref("");
const uploadedFile = ref(null);
const imagePreview = ref(null);
const fileError = ref("");
const emailError = ref("");
const nameError = ref("");
const githubError = ref("");

const fileInput = ref(null);
const emit = defineEmits(['generateTicket']);

const validateForm = () => {
    nameError.value = fullName.value.trim() ? "" : "Full Name is required";
    emailError.value = email.value.trim() ? ( /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email.value) ? "" : "Invalid email format" ) : "Email is required";
    githubError.value = github.value.trim() ? "" : "GitHub username is required";
    fileError.value = uploadedFile.value ? "" : "Avatar upload is required";

    return !nameError.value && !emailError.value && !githubError.value && !fileError.value;
};

const triggerFileUpload = () => {
    fileInput.value.click();
};

const handleFileChange = (event) => {
    const file = event.target.files[0];
    if (file) {
        if (file.size > 500 * 1024) {
            fileError.value = "File size must be less than 500KB";
            return;
        }
        uploadedFile.value = file;
        fileError.value = "";
        const reader = new FileReader();
        reader.onload = () => {
            imagePreview.value = reader.result;
        };
        reader.readAsDataURL(file);
    }
};

const generate = () => {
    if (!validateForm()) return;

    const formData = [{
        fullName: fullName.value,
        email: email.value,
        github: github.value,
        image: imagePreview.value,
        ticketNumber: Math.floor(Math.random() * 1000000),
    }];
    data.value = formData;
    emit('generateTicket', formData);
};
</script>

<style scoped>
.normal-input {
    @apply p-2 border-2 border-neutral-400 bg-neutral-700 rounded-xl;
}
</style>
