<template>
  <div class="login-page">
    <!-- Left Panel -->
    <div class="left-panel">
      <div class="brand">
        <div class="brand-logo">
          <svg width="32" height="32" viewBox="0 0 32 32" fill="none">
            <rect width="32" height="32" rx="8" fill="#0D5C3A"/>
            <path d="M8 22L13 10L16 18L19 14L24 22" stroke="#4ADE80" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <span class="brand-name">LoanFlow</span>
        </div>
      </div>

      <div class="left-content">
        <div class="left-headline">
          <h1>Staff Portal</h1>
          <p>Manage loans, track applicants,<br />and stay in control.</p>
        </div>

        <div class="stats-grid">
          <div class="stat-card">
            <span class="stat-value">₦2.4B</span>
            <span class="stat-label">Total Disbursed</span>
          </div>
          <div class="stat-card">
            <span class="stat-value">1,294</span>
            <span class="stat-label">Active Loans</span>
          </div>
          <div class="stat-card">
            <span class="stat-value">98.2%</span>
            <span class="stat-label">Recovery Rate</span>
          </div>
          <div class="stat-card">
            <span class="stat-value">47</span>
            <span class="stat-label">Pending Review</span>
          </div>
        </div>

        <div class="left-deco">
          <div class="deco-ring ring-1"></div>
          <div class="deco-ring ring-2"></div>
          <div class="deco-ring ring-3"></div>
        </div>
      </div>

      <div class="left-footer">
        <span>© 2024 LoanFlow Inc. All rights reserved.</span>
      </div>
    </div>

    <!-- Right Panel -->
    <div class="right-panel">
      <div class="form-container">
        <!-- Header -->
        <div class="form-header">
          <div class="form-tag">SECURE LOGIN</div>
          <h2>Welcome back</h2>
          <p>Sign in to your staff account to continue</p>
        </div>

        <!-- Alert -->
        <transition name="slide-fade">
          <div v-if="alertMessage" :class="['alert', alertType]">
            <svg v-if="alertType === 'error'" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/>
            </svg>
            <svg v-else width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <polyline points="20 6 9 17 4 12"/>
            </svg>
            {{ alertMessage }}
          </div>
        </transition>

        <!-- Form -->
        <form @submit.prevent="handleLogin" novalidate>
          <!-- Email -->
          <div class="field-group" :class="{ 'has-error': errors.email, 'is-focused': focused === 'email', 'is-filled': form.email }">
            <label for="email">Email Address</label>
            <div class="input-wrapper">
              <svg class="field-icon" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
                <polyline points="22,6 12,13 2,6"/>
              </svg>
              <input
                id="email"
                v-model="form.email"
                type="email"
                placeholder="you@example.com"
                autocomplete="email"
                @focus="focused = 'email'"
                @blur="focused = null; validateEmail()"
              />
            </div>
            <span v-if="errors.email" class="field-error">{{ errors.email }}</span>
          </div>

          <!-- Password -->
          <div class="field-group" :class="{ 'has-error': errors.password, 'is-focused': focused === 'password', 'is-filled': form.password }">
            <label for="password">Password</label>
            <div class="input-wrapper">
              <svg class="field-icon" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                <rect x="3" y="11" width="18" height="11" rx="2" ry="2"/>
                <path d="M7 11V7a5 5 0 0 1 10 0v4"/>
              </svg>
              <input
                id="password"
                v-model="form.password"
                :type="showPassword ? 'text' : 'password'"
                placeholder="Enter your password"
                autocomplete="current-password"
                @focus="focused = 'password'"
                @blur="focused = null; validatePassword()"
              />
              <button type="button" class="toggle-password" @click="showPassword = !showPassword" tabindex="-1">
                <svg v-if="!showPassword" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                  <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/>
                  <circle cx="12" cy="12" r="3"/>
                </svg>
                <svg v-else width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8">
                  <path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"/>
                  <line x1="1" y1="1" x2="23" y2="23"/>
                </svg>
              </button>
            </div>
            <span v-if="errors.password" class="field-error">{{ errors.password }}</span>
          </div>

          <!-- Remember + Forgot -->
          <div class="form-options">
            <label class="checkbox-label">
              <input type="checkbox" v-model="rememberMe" />
              <span class="checkbox-custom"></span>
              <span>Remember me</span>
            </label>
            <a href="#" class="forgot-link">Forgot password?</a>
          </div>

          <!-- Submit -->
          <button type="submit" class="btn-submit" :class="{ loading: isLoading }" :disabled="isLoading">
            <span v-if="!isLoading">Sign In</span>
            <span v-else class="loader-wrap">
              <span class="spinner"></span>
              Signing in...
            </span>
          </button>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import axios from 'axios'

const API_BASE = import.meta.env.VITE_API_URL || 'http://localhost:3030'

const form = reactive({ email: '', password: '' })
const errors = reactive({ email: '', password: '' })
const focused = ref(null)
const showPassword = ref(false)
const rememberMe = ref(false)
const isLoading = ref(false)
const alertMessage = ref('')
const alertType = ref('error')



function fillDemo(cred) {
  form.email = cred.email
  form.password = cred.password
  errors.email = ''
  errors.password = ''
  alertMessage.value = ''
}

function validateEmail() {
  if (!form.email) {
    errors.email = 'Email is required'
  } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.email)) {
    errors.email = 'Enter a valid email address'
  } else {
    errors.email = ''
  }
}

function validatePassword() {
  if (!form.password) {
    errors.password = 'Password is required'
  } else if (form.password.length < 5) {
    errors.password = 'Password is too short'
  } else {
    errors.password = ''
  }
}

async function handleLogin() {
  validateEmail()
  validatePassword()
  if (errors.email || errors.password) return

  isLoading.value = true
  alertMessage.value = ''

  try {
    const { data } = await axios.post(`${API_BASE}/login`, {
      email: form.email,
      password: form.password,
    })

    if (rememberMe.value) {
      localStorage.setItem('access_token', data.data.access_token)
      localStorage.setItem('user', JSON.stringify(data.data.user))
    } else {
      sessionStorage.setItem('access_token', data.data.access_token)
      sessionStorage.setItem('user', JSON.stringify(data.data.user))
    }

    alertType.value = 'success'
    alertMessage.value = `Welcome back, ${data.data.user.name}! Redirecting...`

    setTimeout(() => {
      // router.push('/dashboard')  // uncomment when dashboard exists
    }, 1500)
  } catch (err) {
    alertType.value = 'error'
    alertMessage.value =
      err.response?.data?.message || 'Login failed. Check your credentials.'
  } finally {
    isLoading.value = false
  }
}
</script>

<style scoped>
/* ── Reset & Fonts ── */
* { box-sizing: border-box; }

.login-page {
  display: flex;
  min-height: 100vh;
  font-family: 'Sora', sans-serif;
  background: #f5f4f0;
}

/* ── LEFT PANEL ── */
.left-panel {
  width: 42%;
  background: #0a2e1f;
  display: flex;
  flex-direction: column;
  padding: 40px 48px;
  position: relative;
  overflow: hidden;
}

.brand {
  display: flex;
  align-items: center;
  gap: 12px;
}

.brand-logo {
  display: flex;
  align-items: center;
  gap: 10px;
}

.brand-name {
  font-size: 1.25rem;
  font-weight: 700;
  color: #fff;
  letter-spacing: -0.02em;
}

.left-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 48px;
}

.left-headline h1 {
  font-size: 3rem;
  font-weight: 700;
  color: #ffffff;
  line-height: 1.1;
  letter-spacing: -0.03em;
}

.left-headline p {
  margin-top: 12px;
  font-size: 1rem;
  color: #6b9e7e;
  line-height: 1.6;
}

.stats-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
}

.stat-card {
  background: rgba(255,255,255,0.05);
  border: 1px solid rgba(255,255,255,0.08);
  border-radius: 16px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 6px;
  backdrop-filter: blur(10px);
  transition: background 0.2s;
}

.stat-card:hover {
  background: rgba(255,255,255,0.09);
}

.stat-value {
  font-family: 'DM Mono', monospace;
  font-size: 1.4rem;
  font-weight: 500;
  color: #4ade80;
  letter-spacing: -0.02em;
}

.stat-label {
  font-size: 0.72rem;
  color: #6b9e7e;
  text-transform: uppercase;
  letter-spacing: 0.06em;
}

/* Decorative rings */
.left-deco {
  position: absolute;
  bottom: -80px;
  right: -80px;
  pointer-events: none;
}

.deco-ring {
  position: absolute;
  border-radius: 50%;
  border: 1px solid rgba(74, 222, 128, 0.08);
  transform: translate(-50%, -50%);
}

.ring-1 { width: 240px; height: 240px; top: 0; left: 0; }
.ring-2 { width: 380px; height: 380px; top: 0; left: 0; border-color: rgba(74,222,128,0.05); }
.ring-3 { width: 520px; height: 520px; top: 0; left: 0; border-color: rgba(74,222,128,0.03); }

.left-footer {
  font-size: 0.72rem;
  color: #3a6649;
  margin-top: 32px;
}

/* ── RIGHT PANEL ── */
.right-panel {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 40px 32px;
  background: #f5f4f0;
}

.form-container {
  width: 100%;
  max-width: 440px;
}

.form-header {
  margin-bottom: 32px;
}

.form-tag {
  display: inline-block;
  font-family: 'DM Mono', monospace;
  font-size: 0.65rem;
  letter-spacing: 0.15em;
  color: #0D5C3A;
  background: #d1fae5;
  padding: 4px 10px;
  border-radius: 100px;
  margin-bottom: 16px;
  font-weight: 500;
}

.form-header h2 {
  font-size: 2rem;
  font-weight: 700;
  color: #0f1f15;
  letter-spacing: -0.03em;
  line-height: 1.1;
}

.form-header p {
  margin-top: 8px;
  font-size: 0.9rem;
  color: #6b7280;
}

/* Alert */
.alert {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 12px 16px;
  border-radius: 10px;
  font-size: 0.85rem;
  margin-bottom: 20px;
}

.alert.error {
  background: #fef2f2;
  color: #991b1b;
  border: 1px solid #fecaca;
}

.alert.success {
  background: #f0fdf4;
  color: #14532d;
  border: 1px solid #bbf7d0;
}

/* Field Groups */
.field-group {
  margin-bottom: 20px;
}

.field-group label {
  display: block;
  font-size: 0.82rem;
  font-weight: 600;
  color: #374151;
  margin-bottom: 8px;
  letter-spacing: 0.01em;
}

.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

.field-icon {
  position: absolute;
  left: 14px;
  color: #9ca3af;
  pointer-events: none;
  transition: color 0.2s;
}

.input-wrapper input {
  width: 100%;
  height: 50px;
  padding: 0 44px 0 44px;
  background: #fff;
  border: 1.5px solid #e5e7eb;
  border-radius: 12px;
  font-family: 'Sora', sans-serif;
  font-size: 0.9rem;
  color: #111827;
  transition: border-color 0.2s, box-shadow 0.2s;
  outline: none;
}

.input-wrapper input::placeholder {
  color: #c4c9d4;
}

.field-group.is-focused .input-wrapper input {
  border-color: #0D5C3A;
  box-shadow: 0 0 0 3px rgba(13, 92, 58, 0.1);
}

.field-group.is-focused .field-icon {
  color: #0D5C3A;
}

.field-group.has-error .input-wrapper input {
  border-color: #ef4444;
  box-shadow: 0 0 0 3px rgba(239, 68, 68, 0.08);
}

.field-error {
  display: block;
  margin-top: 6px;
  font-size: 0.75rem;
  color: #ef4444;
}

.toggle-password {
  position: absolute;
  right: 14px;
  background: none;
  border: none;
  cursor: pointer;
  color: #9ca3af;
  display: flex;
  align-items: center;
  padding: 4px;
  transition: color 0.2s;
}

.toggle-password:hover {
  color: #374151;
}

/* Form Options */
.form-options {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 28px;
}

.checkbox-label {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  font-size: 0.85rem;
  color: #374151;
  user-select: none;
}

.checkbox-label input[type="checkbox"] {
  display: none;
}

.checkbox-custom {
  width: 18px;
  height: 18px;
  border: 1.5px solid #d1d5db;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
  flex-shrink: 0;
}

.checkbox-label input:checked + .checkbox-custom {
  background: #0D5C3A;
  border-color: #0D5C3A;
}

.checkbox-label input:checked + .checkbox-custom::after {
  content: '';
  display: block;
  width: 10px;
  height: 6px;
  border-left: 2px solid #fff;
  border-bottom: 2px solid #fff;
  transform: rotate(-45deg) translateY(-1px);
}

.forgot-link {
  font-size: 0.82rem;
  font-weight: 600;
  color: #0D5C3A;
  text-decoration: none;
  transition: opacity 0.2s;
}

.forgot-link:hover { opacity: 0.7; }

/* Submit Button */
.btn-submit {
  width: 100%;
  height: 52px;
  background: #0D5C3A;
  color: #fff;
  border: none;
  border-radius: 12px;
  font-family: 'Sora', sans-serif;
  font-size: 0.95rem;
  font-weight: 600;
  cursor: pointer;
  letter-spacing: 0.01em;
  transition: transform 0.15s, box-shadow 0.15s, background 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.btn-submit:hover:not(:disabled) {
  background: #0a4a2f;
  box-shadow: 0 8px 24px rgba(13, 92, 58, 0.3);
  transform: translateY(-1px);
}

.btn-submit:active:not(:disabled) {
  transform: translateY(0);
}

.btn-submit:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.loader-wrap {
  display: flex;
  align-items: center;
  gap: 8px;
}

.spinner {
  width: 16px;
  height: 16px;
  border: 2px solid rgba(255,255,255,0.3);
  border-top-color: #fff;
  border-radius: 50%;
  animation: spin 0.7s linear infinite;
}

@keyframes spin { to { transform: rotate(360deg); } }

/* Demo hint */
.demo-hint {
  margin-top: 28px;
  padding: 16px;
  background: #f9fafb;
  border: 1px dashed #e5e7eb;
  border-radius: 12px;
}

.demo-title {
  font-size: 0.72rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: #9ca3af;
  font-weight: 600;
  margin-bottom: 10px;
}

.demo-rows {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.demo-row {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px 10px;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.15s;
}

.demo-row:hover { background: #f0fdf4; }

.demo-row svg { margin-left: auto; color: #9ca3af; }

.demo-badge {
  font-family: 'DM Mono', monospace;
  font-size: 0.68rem;
  padding: 2px 8px;
  border-radius: 100px;
  font-weight: 500;
}

.demo-badge.superAdmin { background: #fef3c7; color: #92400e; }
.demo-badge.admin      { background: #ede9fe; color: #4c1d95; }
.demo-badge.staff      { background: #dbeafe; color: #1e3a8a; }

.demo-email {
  font-size: 0.78rem;
  color: #6b7280;
  font-family: 'DM Mono', monospace;
}

/* ── Transition ── */
.slide-fade-enter-active { transition: all 0.3s ease; }
.slide-fade-leave-active { transition: all 0.2s ease; }
.slide-fade-enter-from { transform: translateY(-8px); opacity: 0; }
.slide-fade-leave-to   { opacity: 0; }

/* ── Responsive ── */
@media (max-width: 900px) {
  .login-page { flex-direction: column; }
  .left-panel {
    width: 100%;
    padding: 32px 24px;
    min-height: auto;
  }
  .left-content { gap: 32px; }
  .left-headline h1 { font-size: 2.2rem; }
  .left-deco { display: none; }
  .right-panel { padding: 40px 24px; }
}

@media (max-width: 480px) {
  .stats-grid { grid-template-columns: 1fr 1fr; gap: 10px; }
  .stat-card { padding: 14px; }
  .stat-value { font-size: 1.1rem; }
  .form-container { max-width: 100%; }
}
</style>
