<script lang="ts">

  const units = [
      {
        name: "Genesis",
        sols: 20
      },
      {
        name: "Astrum",
        sols: 23
      },
      {
        name: "Nebula",
        sols: 23
      },
      {
        name: "Nova",
        sols: 23
      },
      {
        name: "Celestia",
        sols: 23
      },
      {
        name: "Zodiac",
        sols: 23
      },
      {
        name: "Twilight",
        sols: 23
      },
      {
        name: "Umbra",
        sols: 23
      },
      {
        name: "Photon",
        sols: 23
      },
      {
        name: "Terminus",
        sols: 19
      }
    ]
	import Footer from './components/Footer.svelte'

  const recalculateTime = (timestamp) => {
    // const rawSeconds = ((new Date).getTime() / 1000) - 1599004800
    const rawSeconds = timestamp - 1599004800
    const seconds = rawSeconds % 62
    const rawMinutes = rawSeconds / 62
    const minutes = rawMinutes % 65
    const rawPhases = rawMinutes / 65
    const phases = rawPhases % 22
    const rawSols = rawPhases / 22

    const solsThisYear = rawSols % 670
    const trimester = solsThisYear <= 223
      ? "Alpha"
      : solsThisYear <= 446
        ? "Beta"
        : "Gamma"

    const solsInThisTrimester = trimester == "Gamma" ? 224 : 223
    
    let solsLeftInTrimester = solsInThisTrimester
    const unit = units.find(({ name, sols }) => {
      console.log({name, sols, solsLeftInTrimester})
      if (solsLeftInTrimester >= sols)
        solsLeftInTrimester -= sols
      else
        return true
    })?.name || "Terminus"

    let sol = solsInThisTrimester
    for (const i in units) {
      if (units[i].name == unit) break
      sol -= units[i].sols
    }

    console.log({solsThisYear, solsInThisTrimester, sol})

    const year = Math.floor(rawSols / 670) + 1

    return {
      sol: sol,
      unit,
      trimester,
      year,
      phases,
      minutes,
      seconds,
      rawSeconds
    }
  }

  let currentTime = recalculateTime(0)

  const getNeighbourInArray = (arr: any[], findFunction: any, direction: 1 | -1) => {
    const foundIndex = arr.findIndex(findFunction)

    if (foundIndex == -1)
      return "null"

    if (direction == -1 && foundIndex == 0)
      return arr[arr.length - 1]

    if (direction == 1 && foundIndex == arr.length - 1)
      return arr[0]

    return arr[foundIndex + direction]
  }

  const minMaxOverflow = (value: number, max: number) => {
    return ('0' + (
      value & (max + 1)
      ).toFixed()).slice(-2)
  }

  window.recalculateTime = t => currentTime = recalculateTime(t)

  // setInterval(() => currentTime = recalculateTime(), 100)
</script>

<main>

  <section>
    <span>{minMaxOverflow(currentTime.phases - 1, 22)}</span>
    <b>{minMaxOverflow(currentTime.phases, 22)}</b>
    <span>{minMaxOverflow(currentTime.phases + 1, 22)}</span>
  </section>
  
  <span>:</span>
  
  <section>
    <span>{minMaxOverflow(currentTime.minutes - 1, 65)}</span>
    <b>{minMaxOverflow(currentTime.minutes, 65)}</b>
    <span>{minMaxOverflow(currentTime.minutes + 1, 65)}</span>
  </section>

  <span>:</span>
  
  <section>
    <span>{minMaxOverflow(currentTime.seconds - 1, 62)}</span>
    <b>{minMaxOverflow(currentTime.seconds, 62)}</b>
    <span>{minMaxOverflow(currentTime.seconds + 1, 62)}</span>
  </section>

  <span></span>
  <span></span>
  <span></span>
  <span></span>
  <span></span>
  <span></span>
  
  <section>
    <!-- TODO: Find which days actually exist  -->
    <span>{(currentTime.sol - 1).toFixed()}</span>
    <b>{(currentTime.sol).toFixed()}</b>
    <span>{(currentTime.sol + 1).toFixed()}</span>
  </section>

  <span>/</span>

  <section>
    <span>{getNeighbourInArray(["Alpha", "Beta", "Gamma"], tri => tri == currentTime.trimester, -1)}</span>
    <b>{currentTime.trimester}</b>
    <span>{getNeighbourInArray(["Alpha", "Beta", "Gamma"], tri => tri == currentTime.trimester,  1)}</span>
  </section>

  <section>
    <span>{getNeighbourInArray(units, ({ name }) => name == currentTime.unit, -1).name}</span>
    <b>{currentTime.unit}</b>
    <span>{getNeighbourInArray(units, ({ name }) => name == currentTime.unit, 1).name}</span>
  </section>

  <span>/</span>

  <section>
    <span>{currentTime.year - 1}</span>
    <b>{currentTime.year}</b>
    <span>{currentTime.year + 1}</span>
  </section>

</main>

<Footer/>

<style lang="scss">
  main {
    // display: flex;
    display: grid;
    grid-auto-flow: column;
    gap: 0.5em;
    justify-content: center;
    align-content: center;

    margin: 2em;
    &:first-child { margin-top: 5em }
    &:last-of-type {
      margin-bottom: 4em;
    }

    & > * {
      display: grid;
      grid-template-columns: 1fr;
      justify-content: center;
      align-content: center;

      font-size: 2em;
    }

    & > span {
      filter: opacity(0.65);
    }
  }

  section span {
    filter: opacity(0.3);
  }

  span, b {
    text-align: center;
  }
</style>