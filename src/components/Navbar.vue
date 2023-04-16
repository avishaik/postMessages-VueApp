<script setup lang="ts">
import SignupForm from "./SignupForm.vue";
import { useModal } from "../composables/modal";
import { useUsers } from "../stores/users";
import { useRouter } from "vue-router";

const modal = useModal();
const userStore = useUsers();
const router = useRouter();

async function logout() {
    await userStore.logout();
    router.push( { path: "/" } );
}
</script>

<template>
    <div class="navbar">
        <div class="navbar-end">
            <div v-if="userStore.currentUserId" class="buttons">
                <RouterLink to="/posts/new" class="button">New Post</RouterLink>
                <div class="button" @click="logout()">Logout</div>
            </div>

            <div v-else class="buttons">
                <div class="button" @click="modal.showModal('signUp')">Sign Up</div>
                <button class="button" @click="modal.showModal('signIn')">Signin</button>
            </div>
        </div>
    </div>

    <Teleport to="#modal">
        <component :is="modal.component.value" />
    </Teleport>
</template>