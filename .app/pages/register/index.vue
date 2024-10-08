<script setup lang="ts">
import { toTypedSchema } from '@vee-validate/zod'
import { Field, useForm } from 'vee-validate'
import { z } from 'zod'

import { AddonInputPassword } from '#components'

definePageMeta({
  layout: 'empty',
  title: 'Signup',
  preview: {
    title: 'Signup 3',
    description: 'For authentication and sign up',
    categories: ['layouts', 'authentication'],
    src: '/img/screens/auth-signup-3.png',
    srcDark: '/img/screens/auth-signup-3-dark.png',
    order: 159,
  },
})

const VALIDATION_TEXT = {
  EMAIL_REQUIRED: 'É necessário um e-mail válido',
  PASSWORD_LENGTH: 'A senha deve conter pelo menos 8 caracteres',
  PASSWORD_CONTAINS_EMAIL: 'A senha não pode conter seu e-mail',
  PASSWORD_MATCH: 'As senhas não coincidem',
}

const passwordRef = ref<InstanceType<typeof AddonInputPassword>>()

// This is the Zod schema for the form input
// It's used to define the shape that the form data will have
const zodSchema = z
  .object({
    email: z.string().email(VALIDATION_TEXT.EMAIL_REQUIRED),
    password: z.string().min(8, VALIDATION_TEXT.PASSWORD_LENGTH),
    confirmPassword: z.string(),
  })
  .superRefine((data: any, ctx: any) => {
    // This is a custom validation function that will be called
    // before the form is submitted
    if (passwordRef.value?.validation?.feedback?.warning || passwordRef.value?.validation?.feedback?.suggestions?.length) {
      ctx.addIssue({
        code: z.ZodIssueCode.custom,
        message: passwordRef.value?.validation?.feedback?.warning || passwordRef.value.validation.feedback?.suggestions?.[0],
        path: ['password'],
      })
    }
    if (data.password !== data.confirmPassword) {
      ctx.addIssue({
        code: z.ZodIssueCode.custom,
        message: VALIDATION_TEXT.PASSWORD_MATCH,
        path: ['confirmPassword'],
      })
    }
  })

// Zod has a great infer method that will
// infer the shape of the schema into a TypeScript type
type FormInput = z.infer<typeof zodSchema>

const validationSchema = toTypedSchema(zodSchema)
const initialValues = {
  email: '',
  password: '',
  confirmPassword: '',
} satisfies FormInput

const { values, handleSubmit, isSubmitting } = useForm({
  validationSchema,
  initialValues,
})

const router = useRouter()
const toaster = useToaster()

// This is where you would send the form data to the server
const onSubmit = handleSubmit(async (values: any) => {
  // here you have access to the validated form values
  console.log('auth-success', values)

  try {
    // fake delay, this will make isSubmitting value to be true
    await new Promise(resolve => setTimeout(resolve, 4000))

    toaster.clearAll()
    toaster.show({
      title: 'Success',
      message: `Account created!`,
      color: 'success',
      icon: 'ph:user-circle-fill',
      closable: true,
    })
  }
  catch (error: any) {
    // handle error

    return
  }

  router.push('/layouts/onboarding-1')
})
</script>

<template>
  <div
    class="bg-muted-100 dark:bg-muted-900 relative min-h-screen w-full overflow-hidden px-4"
  >
    <div
      class="mx-auto flex h-16 w-full max-w-6xl items-center justify-between px-4"
    >
      <NuxtLink
        to="/"
        class="text-muted-400 hover:text-primary-500 dark:text-muted-700 dark:hover:text-primary-500 transition-colors duration-300"
      >
        <TairoLogo class="size-10" />
      </NuxtLink>
      <div>
        <BaseThemeToggle />
      </div>
    </div>
    <div class="flex w-full items-center justify-center">
      <div class="relative mx-auto w-full max-w-2xl">
        <!--Avatars-->
        <img
          class="ltablet:block absolute start-0 top-[30%] hidden size-12 rounded-full object-cover duration-[cubic-bezier(0.86,0,0.07,1)] lg:block"
          src="/img/symbols/usdt-logo.svg"
          alt="Avatar"
          width="48"
          height="48"
        >
        <img
          class="ltablet:block absolute -start-[25%] top-[40%] hidden lg:block"
          src="/img/symbols/solana.svg"
          alt="Avatar"
          width="64"
          height="64"
        >
        <img
          class="ltablet:block absolute -start-[5%] top-[52%] hidden  lg:block"
          src="/img/symbols/floki-logo.svg"
          alt="Avatar"
          width="64"
          height="64"
        >
        <img
          class="ltablet:block absolute -start-[35%] top-[65%] hidden lg:block"
          src="/img/symbols/atom-logo.svg"
          alt="Avatar"
          width="96"
          height="96"
        >
        <img
          class="ltablet:block absolute -start-[35%] top-[20%] hidden lg:block"
          src="/img/symbols/shibi.svg"
          alt="Cripto"
          width="40"
          height="40"
        >
        <img
          class="ltablet:block absolute -start-[55%] top-[40%] hidden  lg:block"
          src="/img/symbols/avax.svg"
          alt="Avatar"
          width="80"
          height="80"
        >

        <img
          class="ltablet:block absolute end-0 top-[30%] hidden lg:block"
          src="/img/symbols/avax.svg"
          alt="Avatar"
          width="48"
          height="48"
        >
        <img
          class="ltablet:block absolute -end-[25%] top-[40%] hidden lg:block"
          src="/img/symbols/icp.svg"
          alt="Avatar"
          width="64"
          height="64"
        >
        <img
          class="ltablet:block absolute -end-[5%] top-[52%] hidden lg:block"
          src="/img/symbols/pepe-logo.svg"
          alt="Avatar"
          width="64"
          height="64"
        >
        <img
          class="ltablet:block absolute -end-[35%] top-[65%] hidden size-24 rounded-full object-cover lg:block"
          src="/img/symbols/btc-logo.svg"
          alt="Avatar"
          width="96"
          height="96"
        >
        <img
          class="ltablet:block absolute -end-[35%] top-[20%] hidden size-10 rounded-full object-cover lg:block"
          src="/img/symbols/chainlink.svg"
          alt="Avatar"
          width="40"
          height="40"
        >
        <img
          class="ltablet:block absolute -end-[55%] top-[40%] hidden  lg:block"
          src="/img/symbols/matic.svg"
          alt="Avatar"
          width="80"
          height="80"
        >

        <!--Form-->
        <div class="me-auto ms-auto mt-4 w-full">
          <form
            method="POST"
            action=""
            class="me-auto ms-auto mt-4 w-full max-w-md"
            novalidate
            @submit.prevent="onSubmit"
          >
            <div class="text-center">
              <BaseHeading
                as="h2"
                size="3xl"
                weight="medium"
              >
                Crie sua conta
              </BaseHeading>
              <BaseParagraph size="sm" class="text-muted-400 mb-6">
                Por favor, entre com seus dados para criar o seu acesso.
              </BaseParagraph>
            </div>
            <div class="p-8">
              <div class="mb-4 space-y-4">
                <Field
                  v-slot="{ field, errorMessage, handleChange, handleBlur }"
                  name="email"
                >
                  <BaseInput
                    :model-value="field.value"
                    :error="errorMessage"
                    :disabled="isSubmitting"
                    type="email"
                    label="E-mail"
                    placeholder="ex: contato@kpax.com.br"
                    icon="lucide:mail"
                    :classes="{
                      input: 'h-12 !ps-12',
                      icon: 'h-12 w-12',
                    }"
                    @update:model-value="handleChange"
                    @blur="handleBlur"
                  />
                </Field>

                <Field
                  v-slot="{ field, errorMessage, handleChange, handleBlur }"
                  name="password"
                >
                  <AddonInputPassword
                    ref="passwordRef"
                    :model-value="field.value"
                    :error="errorMessage"
                    :disabled="isSubmitting"
                    :user-inputs="[values.email ?? '']"
                    label="Senha"
                    placeholder="••••••••••"
                    icon="lucide:lock"
                    :classes="{
                      input: 'h-12 !ps-12',
                      icon: 'h-12 w-12',
                    }"
                    @update:model-value="handleChange"
                    @blur="handleBlur"
                  />
                </Field>

                <Field
                  v-slot="{ field, errorMessage, handleChange, handleBlur }"
                  name="confirmPassword"
                >
                  <BaseInput
                    :model-value="field.value"
                    :error="errorMessage"
                    :disabled="isSubmitting"
                    type="password"
                    label="Confirmar Senha"
                    placeholder="••••••••••"
                    icon="lucide:lock"
                    :classes="{
                      input: 'h-12 !ps-12',
                      icon: 'h-12 w-12',
                    }"
                    @update:model-value="handleChange"
                    @blur="handleBlur"
                  />
                </Field>
              </div>
              <div class="mb-6">
                <div class="mt-6 flex items-center justify-between">
                  <Field
                    v-slot="{ field, errorMessage, handleChange, handleBlur }"
                    name="terms"
                  >
                    <BaseCheckbox
                      :model-value="field.value"
                      :disabled="isSubmitting"
                      :error="errorMessage"
                      rounded="sm"
                      color="primary"
                      @update:model-value="handleChange"
                      @blur="handleBlur"
                    >
                      <span>
                        <span>Eu aceito os</span>
                        <a
                          href="#"
                          class="text-primary-500 hover:underline focus:underline"
                        >
                          Termos de serviço
                        </a>
                      </span>
                    </BaseCheckbox>
                  </Field>
                </div>
              </div>
              <div class="mb-6">
                <BaseButton
                  :disabled="isSubmitting"
                  :loading="isSubmitting"
                  type="submit"
                  color="primary"
                  class="!h-12 w-full"
                >
                  Inscrever-se
                </BaseButton>
              </div>
              <div class="mb-6 grid gap-0 sm:grid-cols-3">
                <hr
                  class="border-muted-200 dark:border-muted-700 mt-3 hidden border-t sm:block"
                >
                <span
                  class="dark:bg-muted-800 text-muted-400 relative top-0.5 bg-white text-center font-sans text-sm"
                >
                  Ou continue com
                </span>
                <hr
                  class="border-muted-200 dark:border-muted-700 mt-3 hidden border-t sm:block"
                >
              </div>
              <!--Social signup-->
              <div class="grid grid-cols-3 gap-2">
                <button
                  type="button"
                  class="bg-muted-200 dark:bg-muted-700 hover:bg-muted-100 dark:hover:bg-muted-600 text-muted-600 dark:text-muted-400 relative inline-flex w-full items-center justify-center rounded px-0 py-3 text-center text-sm font-semibold shadow-sm transition-all duration-300"
                >
                  <Icon name="fa6-brands:google" class="size-5" />
                </button>
                <button
                  type="button"
                  class="bg-muted-200 dark:bg-muted-700 hover:bg-muted-100 dark:hover:bg-muted-600 text-muted-600 dark:text-muted-400 relative inline-flex w-full items-center justify-center rounded px-0 py-3 text-center text-sm font-semibold shadow-sm transition-all duration-300"
                >
                  <Icon name="fa6-brands:twitter" class="size-5" />
                </button>
                <button
                  type="button"
                  class="bg-muted-200 dark:bg-muted-700 hover:bg-muted-100 dark:hover:bg-muted-600 text-muted-600 dark:text-muted-400 relative inline-flex w-full items-center justify-center rounded px-0 py-3 text-center text-sm font-semibold shadow-sm transition-all duration-300"
                >
                  <Icon name="fa6-brands:linkedin-in" class="size-5" />
                </button>
              </div>

              <!--No account link-->
              <p
                class="text-muted-400 mt-4 flex justify-between font-sans text-sm leading-5"
              >
                <span>já tem uma conta?</span>
                <NuxtLink
                  to="/auth/login-3"
                  class="text-primary-600 hover:text-primary-500 font-medium underline-offset-4 transition duration-150 ease-in-out hover:underline"
                >
                  Entrar
                </NuxtLink>
              </p>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
