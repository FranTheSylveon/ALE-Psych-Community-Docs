<script setup lang="ts">
import { computed } from 'vue'
import { useData } from 'vitepress'

type Credit = {
  label: string
  name: string
  url?: string
  note?: string
}

const { frontmatter } = useData<{ credits?: Credit[] }>()

const credits = computed(() => {
  return Array.isArray(frontmatter.value.credits) ? frontmatter.value.credits : []
})
</script>

<template>
  <section v-if="credits.length" class="page-credits">
    <h2>Page credits</h2>
    <div class="grid">
      <article v-for="credit in credits" :key="`${credit.label}-${credit.name}`" class="card">
        <div class="label">{{ credit.label }}</div>
        <div class="name">
          <a
            v-if="credit.url"
            :href="credit.url"
            target="_blank"
            rel="noreferrer"
          >
            {{ credit.name }}
          </a>
          <span v-else>{{ credit.name }}</span>
        </div>
        <p v-if="credit.note" class="note">{{ credit.note }}</p>
      </article>
    </div>
  </section>
</template>

<style scoped>
.page-credits {
  margin-top: 2rem;
  padding-top: 1rem;
  border-top: 1px solid var(--vp-c-divider);
}

.page-credits h2 {
  margin: 0 0 1rem;
  font-size: 1.1rem;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 0.75rem;
}

.card {
  padding: 0.85rem 0.95rem;
  border: 1px solid var(--vp-c-divider);
  border-radius: 12px;
  background: var(--vp-c-bg-soft);
}

.label {
  margin-bottom: 0.35rem;
  color: var(--vp-c-text-2);
  font-size: 0.82rem;
  text-transform: uppercase;
  letter-spacing: 0.04em;
}

.name {
  font-weight: 600;
}

.note {
  margin: 0.45rem 0 0;
  color: var(--vp-c-text-2);
  font-size: 0.92rem;
}
</style>
