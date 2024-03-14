<script setup lang="ts">
import { LoginPayload } from "@/types/index";
import { FormKitNode } from "@formkit/core";
import { nanoid } from 'nanoid'
import axios from "axios";

definePageMeta({
  middleware: ["auth"],
});

async function createLink(payload: LoginPayload, node?:FormKitNode){
  try{
    await axios.post("/links", {
      ...payload,
      short_link: nanoid(8),
    })
    useRouter().push("/links")
  } catch (err){
    handleInvalidForm(err, node)
  }
}
</script>
<template>
  <h1>Create New Link</h1>
  <GoBack>or go back to links</GoBack>
  <!-- <form>
    <label>
      <div>Link</div>
      <input type="text" name="full_link" />
    </label>
    <button class="btn">Create Link</button>
  </form> -->
  <FormKit type="form" submit-label="Create Link" @submit="createLink">
    <FormKit label="Link" type="url" name="full_link"/>
  </FormKit>
</template>
