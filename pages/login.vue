<template>
  <Ucard v-if="!success">
    <template #header> Sign-in to Finance Tracker </template>

    <form>
      <UFormGroup
        label="Email"
        name="email"
        class="mb-4"
        :required="true"
        help="You will recive an email with the confirmation link"
      >
        <UInput type="email" placeholder="Email" required />
      </UFormGroup>

      <UButton
        type="submit"
        variant="solid"
        color="black"
        @click="success = true"
        >Sign-in</UButton
      >
    </form>
  </Ucard>
  <UCard v-else>
    <template #header> Email has been sent </template>

    <div class="text-center">
      <p class="mb-4">
        We have sent an email to <strong>rmsfil@gmail.com</strong> with a link
        to sign-in
      </p>
      <p><strong>Important:</strong> The link will expire in 5 minutes.</p>
    </div>
  </UCard>
</template>
<script setup>
const success = ref(false);
</script>
