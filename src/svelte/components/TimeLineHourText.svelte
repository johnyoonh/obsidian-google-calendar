<script lang="ts">
  import GoogleCalendarPlugin from "../../GoogleCalendarPlugin";
  import {
    getEndHeightOfHour,
    getStartHeightOfHour,
  } from "../../helper/Helper";

  const plugin = GoogleCalendarPlugin.getInstance();
  let hourFormat = plugin.settings.timelineHourFormat;

  export let height = plugin.settings.timelineHeight;
  export let hourRange = [0, 24];

  const switchHourDisplay = () => {
    hourFormat += 1;
    if (hourFormat > 5) {
      hourFormat = 0;
    }
    plugin.settings.timelineHourFormat = hourFormat;
    plugin.saveSettings();
  };

  const getHourText = (hour: number, hourFormat: number): string => {
    const hourMoment = window.moment(`${hour}:00:00`, "H:mm:ss");
    switch (hourFormat) {
      case 0:
        return hourMoment.format("H");
      case 1:
        return hourMoment.format("HH");
      case 2:
        return hourMoment.format("h");
      case 3:
        return hourMoment.format("hh");
      case 4:
        return hourMoment.format("h A");
      case 5:
        return hourMoment.format("hh A");
    }
  };
</script>

<div
  class="gcal-hour-text-container"
  style:margin=" -{getStartHeightOfHour(height, hourRange[0])}px 0px -{getEndHeightOfHour(
    height,
    hourRange[1],
  )}px 0px"
>
  {#each { length: 24 } as _, i}
    <span
      class="gcal-hour-text"
      on:click={switchHourDisplay}
      on:keypress={switchHourDisplay}
      style:height="{height / 24}px">{getHourText(i, hourFormat)}</span
    >
  {/each}
</div>

<style>
  .gcal-hour-text-container {
    display: flex;
    flex-direction: column;
    min-height: 0;
    overflow: hidden;
  }

  .gcal-hour-text {
    font-family: "consolas";
    display: block;
  }
</style>
