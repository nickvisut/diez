<template>
  <docs-detail :tree="tree" :details="details">
    <figure>
      <div :style="{backgroundColor}"></div>
    </figure>
  </docs-detail>
</template>

<script lang="ts">
import {colorToCss} from '@diez/web-sdk-common';
import {Component, Prop, Vue} from 'vue-property-decorator';
import {displayableHsla, displayableRgba, hslToHex} from '../../../../src/color';
type ColorData = import('@diez/prefabs').ColorData;
type DocsTargetSpec = import('@diez/targets').DocsTargetSpec<ColorData>;

/**
 * Color Detail view.
 */
@Component
export default class ColorDetail extends Vue {
  @Prop() readonly tree!: DocsTargetSpec;

  get hsla () {
    const {h, s, l, a} = this.tree.properties;
    return {h: h.value, s: s.value, l: l.value, a: a.value};
  }

  get backgroundColor () {
    return colorToCss(this.hsla);
  }

  get details () {
    return {
      HSLA: displayableHsla(this.hsla),
      RGBA: displayableRgba(this.hsla),
      '8-digit HEX': `#${hslToHex(this.hsla)}`,
      '6-digit HEX': `#${hslToHex(this.hsla).substring(0, 6)}`,
    };
  }
}
</script>
