<template>
  <div class="flex flex-col md:flex-row items-center justify-between">
    <div class="w-1/3 px-2">
      <Ucard v-if="!success">
        <template #header> Sign-in to Finance Tracker </template>

        <form @submit.prevent="handleLogin">
          <UFormGroup label="Email" name="email" class="mb-4" :required="true">
            <UInput type="email" placeholder="Email" v-model="email" required />
          </UFormGroup>

          <UFormGroup
            label="Password"
            name="login_password"
            class="mb-4"
            :required="true"
          >
            <UInput
              type="password"
              placeholder="Password"
              v-model="login_password"
              required
            />
          </UFormGroup>

          <UButton
            type="submit"
            variant="solid"
            color="black"
            :loading="pending[0]"
            :disabled="pending[0]"
            >Sign-in</UButton
          >
        </form>
      </Ucard>
    </div>

    <div class="w-1/3 px-2">
      <Ucard v-if="!success">
        <template #header> Sign-in to Finance Tracker </template>

        <div class="py-4">Register</div>
        <form @submit.prevent="handleRegister">
          <UFormGroup
            label="Email"
            name="register_email"
            class="mb-4"
            :required="true"
          >
            <UInput
              type="email"
              placeholder="Email"
              v-model="register_email"
              required
            />
          </UFormGroup>

          <UFormGroup
            label="Password"
            name="register_password"
            class="mb-4"
            :required="true"
          >
            <UInput
              type="password"
              placeholder="Password"
              v-model="register_password"
              required
            />
          </UFormGroup>

          <UFormGroup
            label="Retype Password"
            name="retype_password"
            class="mb-4"
            :required="true"
          >
            <UInput
              type="password"
              placeholder="Password"
              v-model="retype_password"
              required
            />
          </UFormGroup>

          <UButton
            type="submit"
            variant="solid"
            color="black"
            :loading="pending[1]"
            :disabled="pending[1]"
            >Sign-up</UButton
          >
        </form>
      </Ucard>
    </div>
  </div>
  <UCard v-if="success">
    <template #header> Email has been sent </template>

    <div class="text-center">
      <p class="mb-4">
        We have sent an email to <strong>{{ email }}</strong> with a link to
        sign-in
      </p>
      <p><strong>Important:</strong> The link will expire in 5 minutes.</p>
    </div>
  </UCard>
</template>
<script setup>
const success = ref(false);
const email = ref("");
const login_password = ref("");
const password = ref("");
const register_email = ref("");
const register_password = ref("");
const retype_password = ref("");
const pending = ref([false, false]);
const { toastError, toastSuccess } = useAppToast();
const supabase = useSupabaseClient();

useRedirectIfAuthtenticated();

const handleLogin = async () => {
  pending.value[0] = true;

  try {
    const { data, error } = await supabase.auth.signInWithPassword({
      email: email.value,
      password: login_password.value,
    });

    if (error) {
      toastError({
        title: "Error authenticating",
        description: error.message,
      });
    } else {
      success.value = true;
    }
  } finally {
    pending.value[0] = false;
  }
};

const handleRegister = async () => {
  pending.value[1] = true;
  if (register_password.value !== retype_password.value) return;

  try {
    const { data, error } = await supabase.auth.signUp({
      email: register_email.value,
      password: register_password.value,
      options: {
        emailRedirectTo: "http://localhost:3000",
      },
    });

    if (error) {
      toastError({
        title: "Error with registration",
        description: error.message,
      });
    } else {
      toastSuccess({
        title: "User created successfuly",
      });
    }
  } finally {
    pending.value[1] = false;
  }
};
</script>
