<template lang="pug">
  component.b-link(
    :is="tag"
    :class="classes"
    v-on="events"
    v-bind="attrs"
  )
    span
      slot
      font-awesome-icon.__icon(
        v-if="iconName"
        :icon="iconSettings"
      )
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator'
import { IButtonProps } from '~/components/ui/button.vue'

export interface ILinkProps {
  href: string
  tag?: 'a' | 'button' | 'p' | 'nuxt-link'
  // size?: 's' | 'm' | 'inherit'
  view?: 'primary' | 'secondary'
  underline?: boolean | string
  disabled?: boolean | string
  active?: boolean | string
  iconStyle?: string
  iconName?: string
}

@Component
export default class Link extends Vue {
  @Prop() readonly href!: ILinkProps['href']
  @Prop({ default: 'a' }) readonly tag!: ILinkProps['tag']
  // @Prop({ default: 'm' }) readonly size!: ILinkProps['size']
  @Prop({ default: 'primary' }) readonly view!: ILinkProps['view']
  @Prop({ default: 'fas' }) readonly iconStyle?: IButtonProps['iconStyle']
  @Prop() readonly iconName?: IButtonProps['iconName']
  @Prop() readonly underline!: ILinkProps['underline']
  @Prop() readonly disabled?: ILinkProps['disabled']
  @Prop() readonly active?: ILinkProps['active']

  get iconSettings () {
    return [`${this.iconStyle}`, `${this.iconName}`]
  }

  get notLinkTags (): boolean {
    return this.tag !== 'a'
  }

  get isDisabled (): boolean {
    return typeof this.disabled !== 'undefined' && this.disabled !== false
  }

  get isActive (): boolean {
    return typeof this.active !== 'undefined' && this.active !== false
  }

  get classes (): string[] {
    const classes = [
      `link--view-${this.view}`
      // `link--size-${this.size}`
    ]

    if (this.isDisabled) {
      classes.push('is-disabled')
    }
    if (this.isActive) {
      classes.push('is-active')
    }

    if (this.isUnderline) {
      classes.push('is-underline')
    }

    return classes
  }

  get attrs (): Record<string, unknown> {
    return {
      ...this.tag === 'a' && {
        href: this.href,
        rel: 'noopener noreferrer',
        target: '_blank'
      },
      ...this.tag === 'button' && {
        role: 'button',
        type: 'button'
      },
      ...this.tag === 'nuxt-link' && {
        to: this.href
      }
    }
  }

  get isUnderline (): boolean {
    return typeof this.underline !== 'undefined' && this.underline !== false
  }

  get events (): Record<string, (e: Event) => void> {
    return {
      ...!this.isDisabled && {
        ...this.notLinkTags && {
          click: (e: Event) => this.$emit('click', e)
        },
        mouseenter: (e: Event) => this.$emit('mouseenter', e),
        mouseleave: (e: Event) => this.$emit('mouseleave', e)
      }
    }
  }
}

</script>
