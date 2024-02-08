<script>
  import { onMount } from 'svelte';
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
          <enhanced:img src="$lib/assets/hamburger_menu.svg" alt="An alt text" />
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
</main>

<style>
  @import "../reset.css";

  header {
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: var(--Primary-Colors-primary);
    box-shadow: 0px 5px 5px 0px #0000004D;
    margin-bottom: 10px;
    border-radius: 20px;
    gap: 20px;
    padding:  57px 10px 24px 10px;
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

  main {
    display: flex;
    justify-content: center;
  }

  .calendar {
    display: flex;
    gap: var(--gap);
  }

  .weeks {
    color: var(--Primary-Colors-white-60);
    display: flex;
    flex-direction: column;
    height: 100%;
    justify-content: space-between;
    width: fit-content;
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
</style>
