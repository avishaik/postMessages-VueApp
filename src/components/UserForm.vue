<script setup lang="ts">
import { computed, ref } from "vue";
import FormInput from "../components/FormInput.vue";
import { NewUser } from "../users";
import { validate, length, required } from "../validation";
import { useUsers } from "../stores/users";
import {  useModal } from "../composables/modal";

defineProps<{
    error?: string;
}>();

const emit = defineEmits<{
    (event: 'submit', payload: NewUser): void
}>()

const username = ref('');
const usernameStatus = computed(() => {
  return validate(username.value, [required, length({ min: 5, max: 20})]);
});
const password = ref('');
const passwordStatus = computed(() => {
    return validate(password.value, [required, length({ min: 10, max: 30})]);
});

const isInvalid = computed(() => {
    return (!usernameStatus.value || ! passwordStatus.value.valid);
});

const usersSotre = useUsers();
const modal = useModal();

async function handleSubmit(event: Event) {
    if (isInvalid.value) {
        return;
    }
    const newUser: NewUser = {
        username: username.value,
        password: password.value
    }
    try {
        emit('submit', newUser);
    } catch(e) {

    }
    
}
</script>

<template>
    <form class="form" @submit.prevent="handleSubmit">
        <FormInput name="Username" v-model="username" :status="usernameStatus" type="text" />
        <FormInput name="Password" v-model="password" :status="passwordStatus" type="password" />
        <div v-if="error" class="is-danger help">
            {{ error }}
        </div>
        <button class="button" :disabled="isInvalid">Submit</button>
    </form>
</template>

<style scoped>
.form {
    background: white;
    padding: 30px;
    margin-top: 50px;
}
</style>
