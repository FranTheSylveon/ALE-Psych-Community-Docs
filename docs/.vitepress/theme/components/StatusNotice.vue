<script setup lang="ts">
import { computed } from 'vue'

type StatusKey =
  | 'rapidly-changing'
  | 'newly-added'
  | 'under-review'
  | 'needs-verification'
  | 'version-specific'
  | 'unstable-api'
  | 'breaking-change'
  | 'partial-documentation'
  | 'missing-examples'
  | 'legacy'
  | 'scheduled-for-removal'
  | 'known-issue'
  | 'archived'

type StatusPreset = {
  icon: string
  color: string
  title: string
  description: string
}

const props = defineProps<{
  status: StatusKey
  title?: string
  description?: string
  version?: string
  reason?: string
}>()

const presets: Record<StatusKey, StatusPreset> = {
  'rapidly-changing': {
    icon: '\u21bb',
    color: '#838d86',
    title: 'Rapidly Changing',
    description: 'This page is being updated often and may shift without much notice.'
  },
  'newly-added': {
    icon: '\u2726',
    color: '#5a74d6',
    title: 'Newly Added',
    description: 'This documentation is new and may still be expanding.'
  },
  'under-review': {
    icon: '\u2315',
    color: '#7b5edb',
    title: 'Under Review',
    description: 'This page is being checked for accuracy and may change soon.'
  },
  'needs-verification': {
    icon: '\u26a0',
    color: '#fa005a',
    title: 'Needs Verification',
    description: 'This information should be confirmed against the source or runtime behavior.'
  },
  'version-specific': {
    icon: 'v',
    color: '#86d7dc',
    title: 'Version-Specific',
    description: 'This note applies only to a particular engine version or release line.'
  },
  'unstable-api': {
    icon: '\u2697',
    color: '#ff7f26',
    title: 'Unstable API',
    description: 'The underlying API is still shifting and may not stay compatible.'
  },
  'breaking-change': {
    icon: '\u2716',
    color: '#fa005a',
    title: 'Breaking Change',
    description: 'This behavior differs from older versions in a way that can break existing content.'
  },
  'partial-documentation': {
    icon: '\u25eb',
    color: '#7f8a96',
    title: 'Partial Documentation',
    description: 'Only part of this topic is documented right now.'
  },
  'missing-examples': {
    icon: '{}',
    color: '#5fb39a',
    title: 'Missing Examples',
    description: 'This page needs practical examples before it is fully useful.'
  },
  legacy: {
    icon: '\u25d4',
    color: '#7a7a7a',
    title: 'Legacy',
    description: 'This content describes older behavior that is still worth keeping around.'
  },
  'scheduled-for-removal': {
    icon: '\u23f3',
    color: '#ffc500',
    title: 'Scheduled for Removal',
    description: 'This feature or page is planned to go away in a future update.'
  },
  'known-issue': {
    icon: '\u26a0',
    color: '#fa005a',
    title: 'Known Issue',
    description: 'There is a confirmed problem here that readers should be aware of.'
  },
  archived: {
    icon: '\u2301',
    color: '#838d86',
    title: 'Archived',
    description: 'This page is kept for reference and is no longer actively maintained.'
  }
}

const preset = computed(() => presets[props.status])
const noticeTitle = computed(() => props.title ?? preset.value.title)
const noticeDescription = computed(() => props.description ?? preset.value.description)
const separatorColor = computed(() => preset.value.color)
</script>

<template>
  <aside
    class="status-notice"
    :class="`status-notice--${status}`"
    :style="{ '--notice-separator': separatorColor }"
  >
    <div class="status-notice__icon" aria-hidden="true">
      <span>{{ preset.icon }}</span>
    </div>

    <div class="status-notice__body">
      <div class="status-notice__title-row">
        <b class="status-notice__title">{{ noticeTitle }}</b>
        <span class="status-notice__meta">
          <span v-if="version">Version {{ version }}</span>
          <span v-if="version && reason">·</span>
          <span v-if="reason">{{ reason }}</span>
        </span>
      </div>

      <div class="status-notice__rule" aria-hidden="true" />

      <div class="status-notice__text">
        {{ noticeDescription }}
      </div>
    </div>
  </aside>
</template>

<style scoped>
.status-notice {
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 16px;
  width: 100%;
  padding: 8px 16px;
  margin: 1rem 0 0.25rem;
  box-sizing: border-box;
  background: #7b7b7b3b;
  border: 1px solid color-mix(in srgb, var(--notice-separator) 30%, transparent);
  border-left-width: 6px;
  border-radius: 0;
  font-size: 16px;
  line-height: 160%;
}

.status-notice__icon {
  display: grid;
  place-items: center;
  width: 40px;
  min-width: 40px;
  align-self: start;
  padding-top: 2px;
  text-align: center;
}

.status-notice__icon span {
  display: inline-grid;
  place-items: center;
  width: 40px;
  height: 40px;
  border-radius: 999px;
  background: color-mix(in srgb, var(--notice-separator) 18%, white);
  color: #222;
  font-size: 18px;
  font-weight: 700;
  line-height: 1;
}

.status-notice__body {
  min-width: 0;
}

.status-notice__title-row {
  display: flex;
  flex-wrap: wrap;
  align-items: baseline;
  gap: 8px;
}

.status-notice__title {
  font-size: 1rem;
  font-weight: 700;
}

.status-notice__meta {
  display: inline-flex;
  flex-wrap: wrap;
  gap: 0.35rem;
  color: var(--vp-c-text-2);
  font-size: 0.9rem;
}

.status-notice__rule {
  width: min(200px, 100%);
  height: 4px;
  margin-top: 8px;
  background: var(--notice-separator);
}

.status-notice__text {
  margin-top: 8px;
  font-size: 12px;
  line-height: 1.6;
}

@media (max-width: 640px) {
  .status-notice {
    grid-template-columns: auto 1fr;
    gap: 12px;
    padding: 8px 12px;
  }

  .status-notice__icon,
  .status-notice__icon span {
    width: 36px;
    height: 36px;
    min-width: 36px;
  }

  .status-notice__rule {
    width: 100%;
  }
}
</style>
