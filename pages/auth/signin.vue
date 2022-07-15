<template>
	<v-row align="center" justify="center" style="height: 100vh">
		<v-col cols="12" sm="8" md="4" align="center">
			<v-card
				width="500"
				class="elevation-4 text-left py-2"
				shaped
				color="teal"
				dark
			>
				<v-card-title> Login </v-card-title>
				<v-card-subtitle> Login to your dashboard </v-card-subtitle>
				<v-card-text>
					<v-form>
						<v-text-field
							name="login"
							label="Login"
							prepend-icon="mdi-account"
							type="text"
							v-model="auth.email"
						></v-text-field>
						<v-text-field
							name="password"
							label="Password"
							prepend-icon="mdi-lock"
							type="password"
							v-model="auth.password"
						></v-text-field>
					</v-form>
				</v-card-text>
				<v-card-actions class="text-center">
					<v-btn
						color="green"
						class="login-button"
						@click="login"
						depressed
						large
						>Login</v-btn
					>
					<v-btn
						class="reset-button"
						@click="forgotPassword"
						depressed
						large
						>Forgot Password</v-btn
					>
				</v-card-actions>
			</v-card>
			<v-snackbar
				:timeout="4000"
				v-model="snackbar"
				absolute
				bottom
				outlined
				center
			>
				{{ snackbarText }}
			</v-snackbar>
		</v-col>
	</v-row>
</template>

<script>
export default {
	data: () => ({
		snackbar: false,
		snackbarText: 'No error message',
		auth: {
			email: '',
			password: '',
		},
	}),
	methods: {
		login() {
			this.$fire.auth
				.signInWithEmailAndPassword(this.auth.email, this.auth.password)
				.then((user) => {
					// we are signed in
					$nuxt.$router.push('/')
				})
				.catch((error) => {
					this.snackbarText = error.message
					this.snackbar = true
				})
		},
		forgotPassword() {
			this.$fire.auth
				.sendPasswordResetEmail(this.auth.email)
				.then(() => {
					this.snackbarText = 'reset link sent to ' + this.auth.email
					this.snackbar = true
				})
				.catch((error) => {
					this.snackbarText = error.message
					this.snackbar = true
				})
		},
	},
}
</script>

<style lang="scss" scoped></style>
