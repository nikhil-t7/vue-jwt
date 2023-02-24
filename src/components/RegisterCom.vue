<template>
<h3>Sign Up</h3>
<form @submit.prevent="handleSubmit" novalidate>
    <div class="form-group">
        <p>
            <label for="">First Name</label>
            <input type="text" class="form-control" v-model="state.first_name" placeholder="First Name" />
            <span v-if="v$.first_name.$error" class="text-danger">
                {{ v$.first_name.$errors[0].$message }}
            </span>
        </p>
    </div>

    <div class="form-group">
        <p>
            <label for="">Last Name</label>
            <input type="text" class="form-control" v-model="state.last_name" placeholder="Last Name" />
            <span v-if="v$.last_name.$error" class="text-danger">
                {{ v$.last_name.$errors[0].$message }}
            </span>
        </p>
    </div>

    <div class="form-group">
        <p>
            <label for="">Email</label>
            <input type="email" class="form-control" v-model="state.email" placeholder="Email" />
            <span v-if="v$.email.$error" class="text-danger">
                {{ v$.email.$errors[0].$message }}
            </span>
        </p>
    </div>

    <div class="form-group">
        <p>
            <label for="">Password</label>
            <input type="password" class="form-control" v-model="state.password" placeholder="Password" />
            <span v-if="v$.password.$error" class="text-danger">
                {{ v$.password.$errors[0].$message }}
            </span>
        </p>
    </div>

    <div class="form-group">
        <p>
            <label for="">Confirm Password</label>
            <input type="password" class="form-control" v-model="state.password_confirm" placeholder="Confirm Password" />
            <span v-if="v$.password_confirm.$error" class="text-danger">
                {{ v$.password_confirm.$errors[0].$message }}
            </span>
        </p>
    </div>
    <br />
    <button class="btn btn-primary btn-block">Sign Up</button>

</form>
</template>

<script>
import useValidate from '@vuelidate/core'
import {
    required,
    email,
    minLength,
    maxLength,
    sameAs,
    helpers
} from '@vuelidate/validators'
import {
    reactive,
    computed
} from 'vue'

export default {
    setup() {
        const state = reactive({
            first_name: '',
            last_name: '',
            email: '',
            password: '',
            password_confirm: '',
            submitted : ''
        })

        const containsUppercase = (value) => /[A-Z]/.test(value)
        const containsLowercase = (value) => /[a-z]/.test(value);
        const containsNumber = (value) => /[0-9]/.test(value);
        const containsSpecial = (value) => /[#?!@$%^&*-]/.test(value);

        const rules = computed(() => {
            return {
                first_name: {
                    required : helpers.withMessage(
                        'First Name is required',
                        required
                    )
                },
                last_name: {
                    required : helpers.withMessage(
                        'Last Name is required',
                        required
                    )
                },
                email: {
                    required : helpers.withMessage(
                        'Email is required',
                        required
                    ),
                    email
                },
                password: {
                    required : helpers.withMessage(
                        'Password is required',
                        required
                    ),
                    minLength: minLength(8),
                    maxLength: maxLength(50),
                    containsUppercase : helpers.withMessage(
                        'Password Must contain one Uppercase Character',
                        containsUppercase
                    ),
                    containsLowercase : helpers.withMessage(
                        'Password Must contain one Lowercase Character',
                        containsLowercase
                    ),
                    containsNumber : helpers.withMessage(
                        'Password Must contain one Number',
                        containsNumber
                    ),
                    containsSpecial : helpers.withMessage(
                        'Password Must contain one Special Character',
                        containsSpecial
                    )
                },
                password_confirm: {
                    required : helpers.withMessage(
                        'Confirm Password is required',
                        required
                    ),
                    sameAs: helpers.withMessage(
                        'Confirm Password must be same',
                        sameAs(state.password)
                    )
                }
            }
        })

        const v$ = useValidate(rules, state)
        return {
            state,
            v$
        }
    },
    name: 'RegisterCom',
    methods: {
        async handleSubmit() {
            this.v$.$validate()
            if (!this.v$.$error) {
                alert(" Form submitted...")
                this.$router.push('/login');
            }
        }
    }
}
</script>
