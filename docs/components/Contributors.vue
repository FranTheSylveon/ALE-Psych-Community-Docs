<script setup lang="ts">
import { onMounted, ref } from 'vue'

type Contributor = {
  login: string
  avatar_url: string
  html_url: string
  contributions: number
}

const repoApi = 'https://api.github.com/repos/ALE-Psych-Crew/ALE-Psych-Community-Docs/contributors?per_page=100&anon=1'

const contributors = ref<Contributor[]>([])
const loading = ref(true)
const error = ref('')

onMounted(async () => {
  try {
    const response = await fetch(repoApi, {
      headers: {
        Accept: 'application/vnd.github+json'
      }
    })

    if (!response.ok) {
      throw new Error(`GitHub API returned ${response.status}`)
    }

    contributors.value = await response.json()
  } catch (err) {
    error.value = err instanceof Error ? err.message : 'Failed to load contributors.'
  } finally {
    loading.value = false
  }
})
</script>

<template>
  <section class="contributors">
    <h2>Contributors</h2>
    <p class="intro">Live from GitHub. This list updates automatically from the repository contributors API.</p>

    <p v-if="loading" class="status">Loading contributors...</p>
    <p v-else-if="error" class="status error">
      Could not load contributors: {{ error }}
    </p>

    <div v-else class="grid">
      <a
        v-for="contributor in contributors"
        :key="contributor.login"
        class="card"
        :href="contributor.html_url"
        target="_blank"
        rel="noreferrer"
      >
        <img
          class="avatar"
          :src="contributor.avatar_url"
          :alt="`${contributor.login} profile picture`"
          width="40"
          height="40"
        >
        <div class="meta">
          <div class="login">{{ contributor.login }}</div>
          <div class="count">{{ contributor.contributions }} contribution{{ contributor.contributions === 1 ? '' : 's' }}</div>
        </div>
      </a>
    </div>
  </section>
</template>

<style scoped>
.contributors {
  margin-top: 2rem;
  padding-top: 1rem;
  border-top: 1px solid var(--vp-c-divider);
}

.intro {
  margin: 0 0 1rem;
  color: var(--vp-c-text-2);
  font-size: 0.95rem;
}

.status {
  margin: 0;
  color: var(--vp-c-text-2);
}

.status.error {
  color: var(--vp-c-danger-1);
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 0.75rem;
}

.card {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.8rem 0.9rem;
  border: 1px solid var(--vp-c-divider);
  border-radius: 12px;
  background: var(--vp-c-bg-soft);
  color: inherit;
  text-decoration: none;
  transition: border-color 0.2s ease, transform 0.2s ease, box-shadow 0.2s ease;
}

.card:hover {
  border-color: var(--vp-c-brand-1);
  transform: translateY(-1px);
  box-shadow: 0 8px 20px rgb(0 0 0 / 0.06);
}

.avatar {
  flex: none;
  width: 40px;
  height: 40px;
  border-radius: 999px;
}

.meta {
  min-width: 0;
}

.login {
  font-weight: 600;
  line-height: 1.2;
  word-break: break-word;
}

.count {
  color: var(--vp-c-text-2);
  font-size: 0.9rem;
}
</style>
