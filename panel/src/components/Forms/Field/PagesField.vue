<template>
  <k-field v-bind="$props" class="k-pages-field">
    <k-button-group slot="options" class="k-field-options">
      <k-button
        v-if="more && !disabled"
        icon="add"
        class="k-field-options-button"
        @click="open"
      >
        {{ $t('select') }}
      </k-button>
    </k-button-group>
    <template v-if="selected.length">
      <k-draggable
        :element="elements.list"
        :handle="true"
        :list="selected"
        :data-size="size"
        :data-invalid="isInvalid"
        @end="onInput"
      >
        <component
          v-for="(page, index) in selected"
          :is="elements.item"
          :key="page.id"
          :sortable="!disabled && selected.length > 1"
          :text="page.text"
          :info="page.info"
          :link="page.link"
          :icon="page.icon"
          :image="page.image"
        >
          <k-button
            v-if="!disabled"
            slot="options"
            icon="remove"
            @click="remove(index)"
          />
        </component>
      </k-draggable>
    </template>
    <k-empty
      v-else
      :layout="layout"
      :data-invalid="isInvalid"
      icon="page"
      @click="open"
    >
      {{ empty || $t('field.pages.empty') }}
    </k-empty>
    <k-pages-dialog ref="selector" @submit="select" />
  </k-field>
</template>

<script>
import picker from "@/mixins/picker/field.js";

export default {
  mixins: [picker],
  methods: {
    open() {
      if (this.disabled) {
        return false;
      }

      this.$refs.selector.open({
        endpoint: this.endpoints.field,
        max: this.max,
        multiple: this.multiple,
        search: this.search,
        selected: this.selected.map(page => page.id),
      });
    }
  }
};
</script>

<style lang="scss">
.k-pages-field[data-disabled] * {
  pointer-events: all !important;
}
</style>
