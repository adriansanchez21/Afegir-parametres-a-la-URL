<script setup lang="ts">
import axios, { AxiosError } from "axios";
import { ref, type Ref } from "vue";
import { LoginPayload } from "@/types/index";
import { FormKitNode } from "@formkit/core";

const { login } = useAuth();

const form: Ref<LoginPayload> = ref<LoginPayload>({
  email: "",
  password: "",
});

const login2 = async (form: LoginPayload) => {
  let resLogin;
  try {
    resLogin = await axios.post("/login", form);
    const userInfo = await axios.get("/user");
    console.log(userInfo);
    useRouter().push("/me");
  } catch (e) {
    console.error(`Ha ocurrido un error: ${e}`);
  }
  console.log(resLogin);
};

definePageMeta({
  middleware: ["guest"],
  layout: "centered",
});

const errors = ref({
  email: [],
  password: []
})



//const loginError = ref<string | null>(null);

async function handleLogin(form:LoginPayload, node?:FormKitNode) {
  try {
    await login(form);
  } catch (error) {
    if(error instanceof AxiosError && error.response?.status===422){
      node?.setErrors([], error.response.data.errors)
    }
  }
};
</script>
<template>
  <div class="login">
    <h1>Login</h1>
    <!-- <form @submit.prevent="handleLogin">
      <label>
        <div>Email</div>
        <input v-model="form.email" type="text" />
      </label>

      <label>
        <div>Password</div>
        <input v-model="form.password" type="password" />
      </label>
      <button class="btn">Login</button>
    </form> -->
    
    <FormKit type="form" submit-label="Login" @submit="handleLogin">
      <FormKit label="Email" name="email" type="email" />
      <FormKit label="Password" name="password" type="password" />
    </FormKit>

    <!-- <div v-if="loginError">{{ loginError }}</div> -->

    <p>
      Don't have an account?
      <NuxtLink class="underline text-lime-600" to="/register">Register now!</NuxtLink>
    </p>
  </div>
</template>

