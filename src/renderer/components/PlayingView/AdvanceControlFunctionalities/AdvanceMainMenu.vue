<template>
<base-info-card class="card" ref="cardWidth"
  :borderRadius="7"
  :contentMinHeight="119"
  :contentMinWidth="cardWidth > minInfoCardWidth ? cardWidth : minInfoCardWidth"
  :style="{
    cursor: 'default',
    height: this.readyShow === 'mainMenu' ? menuCardHeight : this.readyShow === 'subMenu' ? subtitleCardHeight : audioCardHeight,
    transition: 'height 100ms linear, width 100ms linear',
    fontWeight: '700',
    letterSpacing: '0.2px',
    width: cardWidth > minInfoCardWidth ? `${cardWidth}px` : `${minInfoCardWidth}px`,
  }">
  <transition :name="this.readyShow === 'mainMenu' ? 'setUp' : 'setUpLeft'">
    <div class="mainItems" v-show="readyShow === 'mainMenu'"
      :style="{
        bottom: readyShow === 'mainMenu' ? '' : '0px',
      }">
      <div class="playSpeed"
        @click.left="handleClick"
        @mouseenter="handleMouseenter(1)"
        @mouseleave="handleMouseleave()"
        :style="{
          height: speedHeight,
          transition: 'height 100ms linear',
        }">
        <transition name="arrow">
          <div class="hoverBack" v-show="!speedChosen && hoverIndex === 1" :style="{ height: speedHeight }"></div>
        </transition>
        <advance-row-items :cardWidth="cardWidth > minInfoCardWidth ? cardWidth : minInfoCardWidth" :ChosenSize="ChosenSize" :isRateMenu="true" :lists="numList" :item="$t('advance.rateTitle')" :size="computedSize" :isChosen="speedChosen" :color="hoverIndex === 1 && !speedChosen ? 'rgba(255, 255, 255, 0.9)' : 'rgba(255, 255, 255, 0.6)'"></advance-row-items>
      </div>
      <div class="subtitleControl"
        @mouseenter="handleMouseenter(2)"
        @mouseleave="handleMouseleave()"
        @click.left="handleSubClick">
        <transition name="arrow">
          <div class="hoverSubBack" v-show="hoverIndex === 2"></div>
        </transition>
        <div class="subContainer"
          :style="{ cursor: 'pointer'}">
          <div class="item2"
            :style="{
              color: hoverIndex === 2 ? 'rgba(255, 255, 255, 0.9)' : 'rgba(255, 255, 255, 0.6)',
              transition: 'color 300ms',
            }">
            <div class="subSettings">{{ this.$t('advance.subMenu') }}</div>
            <transition name="arrow">
              <Icon class="arrowRight" type="rightArrow" v-show="hoverIndex === 2"></Icon>
            </transition>
          </div>
        </div>
      </div>
      <div class="audioItems"
        @mouseenter="handleMouseenter(3)"
        @mouseleave="handleMouseleave()"
        @click.left="handleAudioClick"
        :style="{ cursor: 'pointer' }">
        <transition name="arrow">
          <div class="hoverAudioBack" v-show="hoverIndex === 3"></div>
        </transition>
        <div class="audioContainer">
          <div class="item3"
            :style="{
              color: hoverIndex === 3 ? 'rgba(255, 255, 255, 0.9)' : 'rgba(255, 255, 255, 0.6)',
              transition: 'color 300ms',
            }">
            <div class="audioSettings">{{ this.$t('advance.audioMenu') }}</div>
            <transition name="arrow">
              <Icon class="arrowRight" type="rightArrow" v-show="hoverIndex === 3"></Icon>
            </transition>
          </div>
        </div>
      </div>
    </div>
  </transition>

  <transition :name="this.readyShow === 'mainMenu' ? 'setUp' : 'setUpLeft'">
    <div class="mainItems1" v-show="readyShow === 'subMenu'"
      :style="{
        bottom: readyShow === 'subMenu' ? '' : '0px',
      }">
      <div class="topContainer"
        @click.left="handleSubBackClick"
        @mouseenter="handleSubBackEnter"
        @mouseleave="handleSubBackLeave">
        <div class="topContent">
          <Icon :type="backSubHover ? 'leftArrowHover' : 'leftArrow'"></Icon>
          <div class="text"
            :style="{
              color: backSubHover ? 'rgba(255, 255, 255, 0.4)' : 'rgba(255, 255, 255, 0.2)',
            }">{{ this.$t('advance.subMenu') }}</div>
        </div>
      </div>
      <div class="itemSize" @click.left="handleSizeClick"
        @mouseenter="handleSubMouseenter(1)"
        @mouseleave="handleSubMouseleave()"
        :style="{
          height: subSizeHeight,
          transition: 'height 100ms linear',
        }">
        <transition name="arrow">
          <div class="hoverBack" v-show="!subSizeChosen && hoverSubIndex === 1" :style="{ height: subSizeHeight }"></div>
        </transition>
        <advance-row-items :cardWidth="cardWidth > minInfoCardWidth ? cardWidth : minInfoCardWidth" :ChosenSize="ChosenSize" :isRateMenu="false" :lists="$t('advance.fontItems')" :item="$t('advance.fontSize')" :size="computedSize" :isChosen="subSizeChosen" :color="hoverSubIndex === 1 && !subSizeChosen ? 'rgba(255, 255, 255, 0.9)' : 'rgba(255, 255, 255, 0.6)'"></advance-row-items>
      </div>
      <div class="subtitleStyle" @click.left="handleColorClick"
        @mouseenter="handleSubMouseenter(2)"
        @mouseleave="handleSubMouseleave()"
        :style="{
          height: subColorHeight,
          transition: 'height 100ms linear',
        }">
        <transition name="arrow">
          <div class="hoverBack" v-show="!subColorChosen && hoverSubIndex === 2" :style="{ height: subColorHeight }"></div>
        </transition>
        <advance-color-items :item="$t('advance.fontStyle')" :size="computedSize" :isChosen="subColorChosen" :color="hoverSubIndex === 2 && !subColorChosen ? 'rgba(255, 255, 255, 0.9)' : 'rgba(255, 255, 255, 0.6)'"></advance-color-items>
      </div>
      <div class="subtitleDelay" @click.left="handleDelayClick"
        @mouseenter="handleSubMouseenter(3)"
        @mouseleave="handleSubMouseleave()"
        :style="{
          height: subDelayHeight,
          transition: 'height 100ms linear',
        }">
        <transition name="arrow">
          <div class="hoverBack" v-show="!subDelayChosen && hoverSubIndex === 3 && isSubtitleAvailable" :style="{ height: subDelayHeight }"></div>
        </transition>
        <advance-selected-items :isSubtitleAvaliable="isSubtitleAvailable" :isSubDelay="true" :item="$t('advance.subDelay')" :size="computedSize" :isChosen="subDelayChosen" :color="hoverSubIndex === 3 && !subDelayChosen ? 'rgba(255, 255, 255, 0.9)' : 'rgba(255, 255, 255, 0.6)'"></advance-selected-items>
      </div>
    </div>
  </transition>

  <transition :name="this.readyShow === 'mainMenu' ? 'setUp' : 'setUpLeft'">
    <div class="mainItems2" v-show="readyShow === 'audioMenu'"
      :style="{
        bottom: readyShow === 'audioMenu' ? '' : '0px',
      }">
      <div class="topContainer"
        @click.left="handleAudioBackClick"
        @mouseenter="handleAudioBackEnter"
        @mouseleave="handleAudioBackLeave">
        <div class="topContent">
          <Icon :type="backAudioHover ? 'leftArrowHover' : 'leftArrow'"></Icon>
          <div class="text"
            :style="{
              color: backAudioHover ? 'rgba(255, 255, 255, 0.4)' : 'rgba(255, 255, 255, 0.2)',
            }">{{ this.$t('advance.audioMenu') }}</div>
        </div>
      </div>
      <div class="audioDelay"
        @click.left="1"
        @mouseenter="handleAudioMouseenter(1)"
        @mouseleave="handleAudioMouseleave()"
        :style="{
          height: audioDelayHeight,
          transition: 'height 100ms linear',
        }"><!--disable temporarily-->
        <transition name="arrow">
          <!--<div class="hoverBack" v-show="!showDelay && hoverAudioIndex === 1" :style="{ height: audioDelayHeight }"></div>-->
        </transition>
        <advance-selected-items :isSubDelay="false" :item="$t('advance.audioDelay')" :size="computedSize" :isChosen="showDelay" :color="hoverAudioIndex === 1 && !showDelay ? 'rgba(255, 255, 255, 0.2)' : 'rgba(255, 255, 255, 0.2)'"></advance-selected-items>
      </div>
      <div class="changeTrack" @click.left="handleTrackClick"
        @mouseenter="handleAudioMouseenter(2)"
        @mouseleave="handleAudioMouseleave()"
        :style="{
          height: changeTrackHeight,
          transition: 'height 100ms linear',
        }">
        <transition name="arrow">
          <div class="hoverBack" v-show="!showTrack && hoverAudioIndex === 2" :style="{ height: changeTrackHeight }"></div>
        </transition>
        <div class="trackContainer">
        <transition name="audioTransIn">
          <div class="item2" v-show="!showTrack"
            :style="{ cursor: 'pointer' }">
            <div class="leftTrackTitle advanceNormalTitle" :style="{
              color: hoverAudioIndex === 2 ? 'rgba(255, 255, 255, 0.9)' : 'rgba(255, 255, 255, 0.6)',
              transition: 'color 300ms',
            }">{{ this.$t('advance.changeTrack') }}</div>
            <div class="rightTrackItem advanceNormalItem"
              :style="{
                color: 'rgba(255, 255, 255, 0.6)',
              }">{{ currentAudioTrack }}</div>
          </div>
        </transition>
        </div>
        <transition name="audioTransOut">
          <advance-column-items :item="$t('advance.changeTrack')" :size="computedSize" v-show="showTrack"></advance-column-items>
        </transition>
      </div>
    </div>
  </transition>
</base-info-card>
</template>

<script>
import { mapGetters } from 'vuex';
import AdvanceRowItems from './AdvanceRowItems.vue';
import BaseInfoCard from '../InfoCard.vue';
import Icon from '../../BaseIconContainer.vue';
import AdvanceColorItems from './AdvanceColorItems.vue';
import AdvanceSelectedItemts from './AdvanceSelectItems.vue';
import AdvanceColumnItems from './AdvanceColumnItems.vue';

export default {
  name: 'AdvanceMainMenu',
  data() {
    return {
      numList: [0.5, 1, 1.2, 1.5, 2],
      speedChosen: false,
      rightArrowSub: false,
      rightArrowMed: false,
      preStyle: 'linear-gradient(90deg, rgba(255,255,255,0.00) 0%, rgba(255,255,255,0.045) 20%, rgba(255,255,255,0.00) 78%, rgba(255,255,255,0.00) 100%)',
      hoverIndex: -1,
      readyShow: 'mainMenu',
      backSubHover: false,
      subSizeChosen: false,
      subColorChosen: false,
      subDelayChosen: false,
      hoverSubIndex: -1,
      showDelay: false,
      showTrack: false,
      hoverAudioIndex: -1,
      backAudioHover: false,
      cardWidth: 170,
      normalFont: 'Avenir, Roboto-Regular, PingFang SC, Microsoft Yahei',
    };
  },
  props: {
    clearState: {
      type: Boolean,
    },
  },
  watch: {
    displayLanguage() {
      this.cardWidth = this.maxTextLength + (3 * this.subStyleWidth);
      this.$bus.$emit('card-init-left');
    },
    readyShow() {
      this.cardWidth = this.maxTextLength + (3 * this.subStyleWidth);
      this.$bus.$emit('card-init-left');
    },
    textItemFontSize() {
      this.cardWidth = this.maxTextLength + (3 * this.subStyleWidth);
      this.$bus.$emit('card-init-left');
    },
    clearState(val) {
      this.cardWidth = this.maxTextLength + (3 * this.subStyleWidth);
      this.$bus.$emit('card-init-left');
      if (!val) {
        setTimeout(() => {
          this.readyShow = 'mainMenu';
          this.speedChosen = false;
          this.subSizeChosen = false;
          this.subColorChosen = false;
          this.subDelayChosen = false;
          this.showDelay = false;
          this.showTrack = false;
        }, 150);
      }
    },
    trackNum(val) {
      if (val < 1) {
        this.showTrack = false;
      }
    },
  },
  computed: {
    ...mapGetters(['winWidth', 'currentFirstSubtitleId', 'winHeight', 'rate', 'chosenSize', 'subtitleDelay', 'displayLanguage', 'winRatio']),
    /**
     * @return {string}
     */
    ChosenSize() {
      const compareContent = ['S', 'M', 'L', 'XL'];
      const enContent = ['Small', 'Normal', 'Large', 'Extra Large'];
      return this.$t(`advance.fontItems[${this.chosenSize}]`) === compareContent[this.chosenSize] ? enContent[this.chosenSize] : this.$t(`advance.fontItems[${this.chosenSize}]`);
    },
    minInfoCardWidth() {
      if (this.computedSize >= 289 && this.computedSize <= 480) {
        return 170;
      } else if (this.computedSize >= 481 && this.computedSize < 1080) {
        return 204;
      }
      return 285.6;
    },
    leftTitleToShow() { // 菜单左侧显示的text
      if (this.readyShow === 'audioMenu') {
        return [this.$t('advance.changeTrack'), this.$t('advance.audioDelay'), this.$t('advance.audioMenu')];
      } else if (this.readyShow === 'subMenu') {
        return [this.$t('advance.subDelay'), this.$t('advance.fontSize'), this.$t('advance.fontStyle'), this.$t('advance.subMenu')];
      }
      return [this.$t('advance.rateTitle'), this.$t('advance.subMenu'), this.$t('advance.audioMenu')];
    },
    maxTextLength() { // 不同菜单界面，一行文字加起来最大的长度
      if (this.readyShow === 'audioMenu') {
        const firstLine = this.getTextWidth(`${this.textItemFontSize}px`, this.normalFont, this.leftTitleToShow[0]) +
          this.getTextWidth(`${this.rightItemFontSize}px`, this.normalFont, '0 ms');
        const secondLine = this.getTextWidth(`${this.textItemFontSize}px`, this.normalFont, this.leftTitleToShow[1]) +
          this.getTextWidth(`${this.rightItemFontSize}px`, this.normalFont, this.currentAudioTrack);
        const thirdLine = this.getTextWidth(`${this.rightItemFontSize}px`, this.normalFont, this.leftTitleToShow[2]) +
          this.rightItemFontSize;
        return Math.max(firstLine, secondLine, thirdLine);
      } else if (this.readyShow === 'subMenu') {
        const firstLine = this.getTextWidth(`${this.textItemFontSize}px`, this.normalFont, this.leftTitleToShow[0]) +
          this.getTextWidth(`${this.rightItemFontSize}px`, this.normalFont, this.ChosenSize);
        const secondLine = this.getTextWidth(`${this.textItemFontSize}px`, this.normalFont, this.leftTitleToShow[1]) + this.subStyleWidth;
        const thirdLine = this.getTextWidth(`${this.textItemFontSize}px`, this.normalFont, this.leftTitleToShow[2]) +
          this.getTextWidth(`${this.rightItemFontSize}px`, this.normalFont, `${this.subtitleDelay / 1000} s`);
        const fourthLine = this.getTextWidth(`${this.rightItemFontSize}px`, this.normalFont, this.leftTitleToShow[3]) +
          this.rightItemFontSize;
        return Math.max(firstLine, secondLine, thirdLine, fourthLine);
      }
      const firstLine = this.getTextWidth(`${this.textItemFontSize}px`, this.normalFont, this.leftTitleToShow[0]) +
        this.getTextWidth(`${this.rightItemFontSize}px`, this.normalFont, `${this.rate} x`);
      const secondLine = this.getTextWidth(`${this.textItemFontSize}px`, this.normalFont, this.leftTitleToShow[1]) + this.textItemFontSize;
      const thirdLine = this.getTextWidth(`${this.textItemFontSize}px`, this.normalFont, this.leftTitleToShow[2]) + this.textItemFontSize;
      return Math.max(firstLine, secondLine, thirdLine);
    },
    subStyleWidth() {
      if (this.computedSize >= 289 && this.computedSize <= 480) {
        return 17;
      } else if (this.computedSize >= 481 && this.computedSize < 1080) {
        return 20.4;
      }
      return 28.56;
    },
    rightItemFontSize() {
      if (this.computedSize >= 289 && this.computedSize <= 480) {
        return 11;
      } else if (this.computedSize >= 481 && this.computedSize < 1080) {
        return 13.2;
      }
      return 18.48;
    },
    textItemFontSize() {
      if (this.computedSize >= 289 && this.computedSize <= 480) {
        return 13;
      } else if (this.computedSize >= 481 && this.computedSize < 1080) {
        return 15.6;
      }
      return 21.84;
    },
    computedSize() {
      return this.winRatio >= 1 ? this.winHeight : this.winWidth;
    },
    currentAudioTrack() {
      const track = this.$store.getters.audioTrackList.filter(track => track.enabled)[0];
      if (track) {
        if (track.language === '' || track.language === 'und') {
          return `${this.$t('advance.track')} ${this.$store.getters.audioTrackList.indexOf(track) + 1}`;
        } else if (this.$store.getters.audioTrackList.length === 1) {
          return `${track.language}`;
        }
        return `${track.name}`;
      }
      return this.$t('advance.chosenTrack');
    },
    speedHeight() {
      return this.speedChosen ? `${this.initialSize(74)}px` : `${this.initialSize(37)}px`;
    },
    subSizeHeight() {
      return this.subSizeChosen ? `${this.initialSize(74)}px` : `${this.initialSize(37)}px`;
    },
    subColorHeight() {
      return this.subColorChosen ? `${this.initialSize(74)}px` : `${this.initialSize(37)}px`;
    },
    subDelayHeight() {
      return this.subDelayChosen ? `${this.initialSize(74)}px` : `${this.initialSize(37)}px`;
    },
    audioDelayHeight() {
      return this.showDelay ? `${this.initialSize(74)}px` : `${this.initialSize(37)}px`;
    },
    changeTrackHeight() {
      return this.showTrack ? `${this.initialSize(this.trackHeight)}px` : `${this.initialSize(37)}px`;
    },
    menuCardHeight() {
      return this.speedChosen ? `${this.initialSize(164)}px` : `${this.initialSize(127)}px`;
    },
    subtitleCardHeight() {
      return !this.subColorChosen && !this.subSizeChosen && !this.subDelayChosen ? `${this.initialSize(156)}px` : `${this.initialSize(193)}px`;
    },
    audioCardHeight() {
      if (this.showDelay) {
        return `${this.initialSize(156)}px`;
      } else if (this.showTrack) {
        return `${this.initialSize(this.containerHeight)}px`;
      }
      return `${this.initialSize(119)}px`;
    },
    isSubtitleAvailable() {
      return this.currentFirstSubtitleId !== '';
    },
    trackNum() {
      return this.$store.getters.audioTrackList.length;
    },
    containerHeight() {
      if (this.trackNum <= 2) {
        return 133 + (this.trackNum * 27) + ((this.trackNum - 1) * 5);
      }
      return 230;
    },
    trackHeight() {
      if (this.trackNum <= 2) {
        return (51 + (this.trackNum * 27)) + ((this.trackNum - 1) * 5);
      }
      return 142;
    },
  },
  components: {
    'base-info-card': BaseInfoCard,
    'advance-row-items': AdvanceRowItems,
    'advance-color-items': AdvanceColorItems,
    'advance-selected-items': AdvanceSelectedItemts,
    'advance-column-items': AdvanceColumnItems,
    Icon,
  },
  methods: {
    initialSize(size) {
      if (this.computedSize >= 289 && this.computedSize <= 480) {
        return size;
      } else if (this.computedSize >= 481 && this.computedSize < 1080) {
        return size * 1.2;
      }
      return size * 1.2 * 1.4;
    },
    handleClick() {
      this.speedChosen = true;
    },
    handleSubClick() {
      this.readyShow = 'subMenu';
      this.speedChosen = false;
    },
    handleAudioClick() {
      this.readyShow = 'audioMenu';
      this.speedChosen = false;
    },
    handleMouseenter(index) {
      this.hoverIndex = index;
    },
    handleMouseleave() {
      this.hoverIndex = -1;
    },
    handleSubBackClick() {
      this.readyShow = 'mainMenu';
      this.subSizeChosen = false;
      this.subDelayChosen = false;
      this.subColorChosen = false;
    },
    handleSubBackEnter() {
      this.backSubHover = true;
    },
    handleSubBackLeave() {
      this.backSubHover = false;
    },
    handleSubMouseenter(index) {
      this.hoverSubIndex = index;
    },
    handleSubMouseleave() {
      this.hoverSubIndex = -1;
    },
    handleSizeClick() {
      this.hoverSubIndex = -1;
      this.subSizeChosen = true;
      this.subDelayChosen = false;
      this.subColorChosen = false;
    },
    handleColorClick() {
      this.hoverSubIndex = -1;
      this.subColorChosen = true;
      this.subSizeChosen = false;
      this.subDelayChosen = false;
    },
    handleDelayClick() {
      if (this.isSubtitleAvailable) {
        this.hoverSubIndex = -1;
        this.subDelayChosen = true;
        this.subSizeChosen = false;
        this.subColorChosen = false;
      }
    },
    handleAudioBackEnter() {
      this.backAudioHover = true;
    },
    handleAudioBackLeave() {
      this.backAudioHover = false;
    },
    handleAudioBackClick() {
      this.readyShow = 'mainMenu';
      this.showDelay = false;
      this.showTrack = false;
    },
    handleAudioMouseenter(index) {
      this.hoverAudioIndex = index;
    },
    handleAudioMouseleave() {
      this.hoverAudioIndex = -1;
    },
    handleAudioDelayClick() {
      this.showDelay = true;
      this.showTrack = false;
    },
    handleTrackClick() {
      if (this.trackNum > 0) {
        this.showDelay = false;
        this.showTrack = true;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
@media screen and (max-aspect-ratio: 1/1) and (min-width: 289px) and (max-width: 480px), screen and (min-aspect-ratio: 1/1) and (min-height: 289px) and (max-height: 480px) {
  .playSpeed, .hoverBack {
    width: 100%;
  }
  .subtitleControl, .audioItems, .topContainer, .itemSize, .subtitleStyle, .subtitleDelay, .audioDelay, .hoverSubBack, .subContainer, .hoverAudioBack, .audioContainer, .trackContainer {
    width: 100%;
    height: 37px;
  }
  .item2, .item3 {
    font-size: 13px;
    width: 100%;
    height: 18px;
    .subSettings, .audioSettings, .leftTrackTitle {
      margin: auto auto auto 17px;
    }
    .arrowRight, .rightTrackItem {
      margin: auto 17px auto auto;
    }
  }
  .topContent {
    width: auto;
    height: 12px;
    margin: auto 9px;
  }
  .text {
    font-size: 11px;
    line-height: 13px;
    margin-left: 3px;
  }
  .setUp-enter-active {
    animation: showP1 .2s;
  }
  .setUp-enter, .setUp-leave-to {
    opacity: 0;
  }
  .setUp-leave-active {
    animation: hideP1 .2s;
  }

  .setUpLeft-enter-active {
    animation: showLeftP1 .2s;
  }
  .setUpLeft-enter, .setUpLeft-leave-to {
    opacity: 0;
  }
  .setUpLeft-leave-active {
    animation: hideLeftP1 .2s;
  }
}
@media screen and (max-aspect-ratio: 1/1) and (min-width: 481px) and (max-width: 1080px), screen and (min-aspect-ratio: 1/1) and (min-height: 481px) and (max-height: 1080px) {
  .playSpeed, .hoverBack {
    width: 100%
  }
  .subtitleControl, .audioItems, .topContainer, .itemSize, .subtitleStyle, .subtitleDelay, .audioDelay, .hoverSubBack, .subContainer, .hoverAudioBack, .audioContainer, .trackContainer {
    width: 100%;
    height: 44.4px;
  }
  .item2, .item3 {
    font-size: 15.6px;
    width: 100%;
    height: 22px;
    .subSettings, .audioSettings, .leftTrackTitle {
      margin: auto auto auto 20.4px;
    }
    .arrowRight, .rightTrackItem {
      margin: auto 20.4px auto auto;
    }
  }
  .topContent {
    width: auto;
    height: 14.4px;
    margin: auto 10.8px;
  }
  .text {
    font-size: 13.2px;
    line-height: 15.6px;
    margin-left: 3.6px;
  }
  .setUp-enter-active {
    animation: showP2 .2s;
  }
  .setUp-enter, .setUp-leave-to {
    opacity: 0;
  }
  .setUp-leave-active {
    animation: hideP2 .2s;
  }
  .setUpLeft-enter-active {
    animation: showLeftP2 .2s;
  }
  .setUpLeft-enter, .setUpLeft-leave-to {
    opacity: 0;
  }
  .setUpLeft-leave-active {
    animation: hideLeftP2 .2s;
  }
}
@media screen and (max-aspect-ratio: 1/1) and (min-width: 1080px), screen and (min-aspect-ratio: 1/1) and (min-height: 1080px) {
  .playSpeed, .hoverBack {
    width: 100%
  }
  .subtitleControl, .audioItems, .topContainer, .itemSize, .subtitleStyle, .subtitleDelay, .audioDelay, .hoverSubBack, .subContainer, .hoverAudioBack, .audioContainer, .trackContainer {
    height: 62.16px;
    width: 100%;
  }
  .item2, .item3 {
    font-size: 21.84px;
    width: 100%;
    height: 30px;
    .subSettings, .audioSettings, .leftTrackTitle {
      margin: auto auto auto 28.48px;
    }
    .arrowRight, .rightTrackItem {
      margin: auto 28.48px auto auto;
    }
  }
  .topContent {
    width: auto;
    height: 20.16px;
    margin: auto 15.12px;
  }
  .text {
    font-size: 18.48px;
    line-height: 21.84px;
    margin-left: 5.04px;
  }
  .setUp-enter-active {
    animation: showP3 .2s;
  }
  .setUp-enter, .setUp-leave-to {
    opacity: 0;
  }
  .setUp-leave-active {
    animation: hideP3 .2s;
  }
  .setUpLeft-enter-active {
    animation: showLeftP3 .2s;
  }
  .setUpLeft-enter, .setUpLeft-leave-to {
    opacity: 0;
  }
  .setUpLeft-leave-active {
    animation: hideLeftP3 .2s;
  }

}

.card {
  -webkit-app-region: no-drag;
}
.hoverBack {
  background-image: linear-gradient(90deg, rgba(255,255,255,0.00) 0%, rgba(255,255,255,0.045) 20%, rgba(255,255,255,0.00) 78%, rgba(255,255,255,0.00) 100%);
}
.mainItems {
  display: flex;
  flex-direction: column;
  position: absolute;
  width: 100%;
  .playSpeed {
    display: flex;
    margin-top: 8px;
  }
  .subtitleControl {
    display: flex;
    .hoverSubBack {
      background-image: linear-gradient(90deg, rgba(255,255,255,0.00) 0%, rgba(255,255,255,0.045) 20%, rgba(255,255,255,0.00) 78%, rgba(255,255,255,0.00) 100%);
    }
    .subContainer {
      position: absolute;
      display: flex;
    }
    .item2 {
      margin: auto;
      display: flex;
      justify-content: space-between;
    }
  }
  .audioItems {
    display: flex;
    .audioContainer {
      position: absolute;
      display: flex;
    }
    .hoverAudioBack {
      background-image: linear-gradient(90deg, rgba(255,255,255,0.00) 0%, rgba(255,255,255,0.045) 20%, rgba(255,255,255,0.00) 78%, rgba(255,255,255,0.00) 100%);
    }
    .item3 {
      margin: auto;
      display: flex;
      justify-content: space-between;
    }
  }
}

.mainItems1 {
  display: flex;
  flex-direction: column;
  position: absolute;
  width: 100%;
  .leftItem {
    margin-top: 1px;
    font-size: 13px;
  }
  .rightItem {
    font-size: 11px;
  }
  .topContainer {
    display: flex;
    cursor: pointer;
    .topContent {
      display: flex;
      justify-content: flex-start;
    }
  }
  .itemSize {
    display: flex;
  }
  .subtitleStyle {
    display: flex;
  }
  .subtitleDelay {
    display: flex;
  }
}

.mainItems2 {
  display: flex;
  flex-direction: column;
  position: absolute;
  width: 100%;
  .topContainer {
    display: flex;
    cursor: pointer;
    .topContent {
      display: flex;
      justify-content: flex-start;
    }
  }
  .trackContainer {
    display: flex;
    position: absolute;
  }
  .audioDelay {
    display: flex;
  }
  .changeTrack {
    display: flex;
    .item2 {
      display: flex;
      justify-content: space-between;
      margin: auto;
    }
  }
}

.arrow-enter-active, .arrow-leave-active {
  transition: opacity .2s;
}
.arrow-enter, .arrow-leave-to {
  opacity: 0;
}

.audioTransOut-leave-active {
  transition-delay: 80ms;
}
.audioTransOut-enter, .audioTrans-leave-to {
  opacity: 0;
}
.audioTransIn-enter-active {
  transition-delay: 80ms;
}
.audioTransIn-enter, .audioTransIn-leave-to {
  opacity: 0;
}

@keyframes showP1 {
  0% {
    opacity: 0;
    right: 170px;
  }
  100% {
    opacity: 1;
    right: 0;
  }
}
@keyframes hideP1 {
  0% {
    opacity: 1;
    right: 0;
  }
  100% {
    opacity: 0;
    right: -170px;
  }
}
@keyframes showP2 {
  0% {
    opacity: 0;
    right: 204px;
  }
  100% {
    opacity: 1;
    right: 0;
  }
}
@keyframes hideP2 {
  0% {
    opacity: 1;
    right: 0;
  }
  100% {
    opacity: 0;
    right: -204px;
  }
}
@keyframes showP3 {
  0% {
    opacity: 0;
    right: 285.6px;
  }
  100% {
    opacity: 1;
    right: 0;
  }
}
@keyframes hideP3 {
  0% {
    opacity: 1;
    right: 0;
  }
  100% {
    opacity: 0;
    right: -285.6px;
  }
}
@keyframes showLeftP1 {
  0% {
    opacity: 0;
    right: -170px;
  }
  100% {
    opacity: 1;
    right: 0;
  }
}
@keyframes hideLeftP1 {
  0% {
    opacity: 1;
    right: 0;
  }
  100% {
    opacity: 0;
    right: 170px;
  }
}
@keyframes showLeftP2 {
  0% {
    opacity: 0;
    right: -204px;
  }
  100% {
    opacity: 1;
    right: 0;
  }
}
@keyframes hideLeftP2 {
  0% {
    opacity: 1;
    right: 0;
  }
  100% {
    opacity: 0;
    right: 204px;
  }
}
@keyframes showLeftP3 {
  0% {
    opacity: 0;
    right: -285.6px;
  }
  100% {
    opacity: 1;
    right: 0;
  }
}
@keyframes hideLeftP3 {
  0% {
    opacity: 1;
    right: 0;
  }
  100% {
    opacity: 0;
    right: 285.6px;
  }
}
</style>
