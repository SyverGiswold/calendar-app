<script>
  import { onMount } from 'svelte';

  let showOverlay = false;

  let startDate = null;
  let startTime = null;
  let endDate = null;
  let endTime = null;

  let date = new Date();
  let daysInMonth;
  let month;
  let year;
  let weeks = [];
  let today = date.getDate();

  // Norwegian abbreviated month names
  let monthNames = ['Januar', 'Februar', 'Mars', 'April', 'Mai', 'Juni', 'Juli', 'August', 'September', 'Oktober', 'November', 'Desember'];

  function getWeekNumber(d) {
    d = new Date(Date.UTC(d.getFullYear(), d.getMonth(), d.getDate()));
    d.setUTCDate(d.getUTCDate() + 4 - (d.getUTCDay() || 7));
    let yearStart = new Date(Date.UTC(d.getUTCFullYear(), 0, 1));
    let weekNo = Math.ceil((((d - yearStart) / 86400000) + 1) / 7);
    return weekNo;
  }

  onMount(() => {
    month = date.getMonth();
    year = date.getFullYear();
    daysInMonth = new Date(year, month + 1, 0).getDate();
    for (let i = 1; i <= daysInMonth; i++) {
      let weekNo = getWeekNumber(new Date(year, month, i));
      if (!weeks.includes(weekNo)) {
        weeks = [...weeks, weekNo]; // make weeks reactive
      }
    }
  });
</script>

<header>
    <div class="month_and_nav">
      <h1 class="month">
        {monthNames[month]}
      </h1>
      <nav>
        <button>
          <enhanced:img src="$lib/assets/hamburger_menu.svg" alt="Åpne meny" />
        </button>
      </nav>
    </div>
    <div class="day_indicator">
      <span>M</span>
      <span>T</span>
      <span>O</span>
      <span>T</span>
      <span>F</span>
      <span>L</span>
      <span>S</span>
    </div>
</header>


<main>
  <div class="calendar">

    <div class="weeks">
      {#each weeks as week}
        <div class="week">
          {week}
        </div>
      {/each}
    </div>

    <div class="days">
      {#each Array(daysInMonth).fill() as _, i (i)}
        <div class={i + 1 === today ? 'day today' : 'day'}>
          {i + 1}
        </div>
      {/each}
    </div>

    <div style="color: transparent !important;" class="weeks">
      {#each weeks as week}
        <div class="week">
          {week}
        </div>
      {/each}
    </div>

  </div>

  <button on:click={() => showOverlay = !showOverlay} class="add_event_button">
    <enhanced:img src="$lib/assets/add_FILLF1F1F1_wght400_GRAD0_opsz24.svg" alt="Legg til Aktivitet"/>
  </button>
</main>

<div class="all_overlays">
<section class="add_event_overlay" class:active={showOverlay}>
  
  <input type="text" placeholder="Tittel">

  
  <div>
    <fieldset class="all_day_container">
    <label>

      <span class="label">Hele dagen</span>

      <input type="checkbox" role="switch">

      <span class="state">
        <span class="container">
          <span class="position"> </span>
        </span>
      </span>

    </label>
  </fieldset>
  
  <input type="date" placeholder="I dag" />
  <input type="time" value="00:00" />
  
  <input type="date" placeholder="I dag" />
  <input type="time" value="00:00" /> 
</div>

<div class="location_input"><enhanced:img src="$lib/assets/Pin.svg" alt=""/> <input placeholder="Adresse" type="text"></div>



</section>
</div>

<footer>

</footer>

<style>
  @import "../reset.css";

  header {
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: var(--Primary-Colors-primary);
    box-shadow: 0px 5px 5px 0px #0000004D;
    border-radius: var(--border-radius);
    gap: 20px;
    padding: 24px 10px 24px 10px;
  }

  .month {
    font-size: var(--h1);
  }

  main {
    display: flex;
    flex-direction: column;
    padding: 10px;
  }

  .day_indicator {
    display: grid;
    grid-template-columns: repeat(7, 46px);
    gap: var(--gap);
    text-align: center;
  }

  .month_and_nav {
    width: 100%;
    display: flex;
    justify-content: space-between;
  }

  .calendar {
    display: flex;
    gap: var(--gap);
    justify-content: center;
  }

  .weeks {
    color: var(--Primary-Colors-white-60);
    display: flex;
    flex-direction: column;
    height: 100%;
    justify-content: space-between;
    width: fit-content;
    gap: var(--gap);
  }

  .days {
    display: grid;
    grid-template-columns: repeat(7, 46px);
    gap: var(--gap);
  }

  .week {
    text-align: end;
    font-size: 12px;
    height: 46px;
  }

  .day {
    width: 46px;
    height: 46px;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: var(--Primary-Colors-secondary);
    border-radius: 7px;
    box-shadow: 0px 5px 5px 0px #0000004D;
  }

  .today {
    color: var(--Color-Sat-red);
    border: 1px solid var(--Color-Sat-red);
  }

  .add_event_button {
    margin-left: auto;
    height: 49px;
    width: 49px;
    background-color: var(--Primary-Colors-secondary);
    box-shadow: 0px 5px 5px 0px #0000004D;
    border-radius: 100vmax;
    padding: 5px;
  }

  .add_event_button img {
    height: 100%;
    width: 100%;
  }

  .all_overlays {
    pointer-events: none;
    overflow-y: hidden;
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
  }

  .all_overlays > * {
    min-height: 100%;
    max-height: 100%;
    overflow: auto;
  }
  .all_overlays * {
    pointer-events: all;
  }

  .add_event_overlay {
    position: absolute;
    top: 100vh;
    left: 0;
    display: flex;
    visibility: hidden;
    flex-direction: column;
    gap: var(--gap);
    background-color: var(--Primary-Colors-secondary);
    width: 100%;
    border-radius: 20px 20px 0 0;
    padding: 24px 10px 24px 10px;
    transition: top 0.3s ease, visibility 0s ease 0.3s;
  }

  .add_event_overlay.active {
    visibility: visible;
    top: 0;
    transition: top 0.3s ease, visibility 0s ease;
  }

  .add_event_overlay * {
    color: var(--bw-white);
  }
  .add_event_overlay > * {
    min-height: 73px;
    background-color: var(--Primary-Colors-primary);
    border-radius: var(--border-radius);
    padding: 20px 10px 20px 10px;
    width: 100%;
    box-shadow: 0px 5px 5px 0px #0000004D;
  }

  .add_event_overlay input {
    font-size: var(--h2);
  }
  ::placeholder {
    color: var(--bw-white);
  }

  .all_day_container label {
    display: flex;
    justify-content: space-between;
    align-items: center;
    min-height: 44px;
  }

  .all_day_container label input[role="switch"] {
    opacity: 0;
  }

  .all_day_container label input[role="switch"] ~ .state {
    display: inline-block;
    user-select: none;
  }

  .all_day_container label input[role="switch"] ~ .state > .container {
    position: relative;
    background-color: var(--Primary-Colors-secondary);
    top: 2px;
    display: inline-block;
    width: 51px;
    height: 29px;
    border-radius: 100vmax;
    box-shadow: 0px 5px 5px 0px #0000004D;
    transition: background-color 0.3s ease;
  }

  .all_day_container label input[role="switch"]:checked ~ .state > .container {
    background-color: var(--Color-Sat-green);
  }

  .all_day_container label input[role="switch"] ~ .state > .container > .position {
    position: relative;
    top: 0;
    left: 0;
    display: block;
    border-radius: 100vmax;
    width: 29px;
    height: 29px;
    background: var(--bw-white);
    transition: left 0.3s ease;
  }

  .all_day_container label input[role="switch"]:checked ~ .state > .container > .position {
    left: calc(100% - 29px);
  }

  .location_input {
    display: flex;
    gap: 20px;
  }

</style>
