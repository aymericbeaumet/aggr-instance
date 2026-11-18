---
title: A Perfect SimCity
link: https://www.jeffquast.com/post/a-perfect-simcity/
source: hnrss-org
published: 2026-11-15T05:54:22Z
updated: 2026-11-15T05:54:22Z
first_seen: 2026-11-18T17:57:41.978411823Z
authors:
- mariuz
content: extracted
html: 2026-11-15-a-perfect-simcity.html
remote_preview:
  url: https://www.jeffquast.com/img/avatar.jpg
  alt: Jeff Quast
extra:
  content:interactive: true
---

##### 22. August 2026

## A Perfect SimCity

After 35 years of playing SimCity, I still could not say what makes a "Perfect" city. An evaluation screen displays a Score of 0 to 1000, and I struggled to reach 900 and never understood its factors. Discovering these factors by play is the fun of a simulation game, and this one has the advantage that the source code is also available.

[Don Hopkins](https://donhopkins.medium.com/) has been maintaining FOSS ports of [SimCity (1989)](https://archive.org/details/msdos_SimCity_1989) as [Micropolis](https://github.com/SimHacker/micropolis) and [MicropolisCore](https://github.com/SimHacker/MicropolisCore), faithfully reproducing the ruleset of the original. Using the Python shims provided by [gym-city](https://github.com/smearle/gym-city) I have run many experiments and share these results here, presenting five high-scoring and five high-population cities, designed algorithmically using the strategies shared below.

 Speed

Figure 1. Ten algorithmically generated cities, 5 with highest score (bigscore1 through 5), and 5 for large population of 700k (bigpop1 through 5).

## Citizens Demand Fewer Roads

The population of a perfectly scoring city is 12-16k. Their population level, crime rate, pollution and land value are perfectly balanced to sustain a 1% Tax Rate. Their roads and rails are few and non-contiguous. A perfectly scoring city is without any fire department, and `bigscore2` through `5` have no police station.

| city      | score | population | crime | pollution | land value | unemployment | cash / year |
| --------- | ----- | ---------- | ----- | --------- | ---------- | ------------ | ----------- |
| bigscore1 | 949.5 | 16,560     | 13    | 36        | 126        | -93          | +6          |
| bigscore2 | 940.0 | 18,400     | 22    | 41        | 124        | -99          | +147        |
| bigscore3 | 940.0 | 12,440     | 22    | 35        | 122        | -96          | +93         |
| bigscore4 | 939.0 | 12,400     | 16    | 30        | 128        | -95          | +86         |
| bigscore5 | 937.0 | 14,640     | 20    | 41        | 127        | -98          | +117        |

Travel requirements of residential and industrial zones are simple. They check their twelve surrounding tiles for any road or rail to decide if it should grow. If none is there, the zone declines.

Commercial zones require a delivery, a vehicle starts on the perimeter road and walks up to 30 steps, arriving the moment it stands beside a developed tile. *The zone's own tiles count*, so any loop of track under 30 steps satisfies it. Commercial zones are placed in the city center because the engine provides a *downtown bonus*, starting at +64 and falling off to a -64 penalty in the far corners of the map.

The [PC SimCity manual](https://archive.org/details/simcity_ibm_manual) recommends rails and it is the most obvious strategy that players discover on their own. Roads carry traffic and emit pollution, which suppresses residential growth. Rails emit no pollution and register no traffic at all.

Roads and rails incur an annual cost, and Police and Fire are very large budget items, they severely impact score if they are under-funded and so must be at 100% funding. Having neither departments is the lowest annual cost allowing for the lowest Tax Rate. Raising the Tax Rate to 2% lowers the score almost 30 points, through direct and indirect factors.

## Citizens Demand More Citizens

In the [Super Nintendo edition](https://en.wikipedia.org/wiki/SimCity_\(1989_video_game\)#Super_NES) the game rewards *population*, the final reward of Megapolis is issued at a population of 500k. Casual players report cities of 600k to 700k on the SNES platform, with [one player reporting 911.2k](https://gamefaqs.gamespot.com/boards/588657-simcity/79143601).

911.2k is probably out of reach for [Micropolis](https://github.com/SimHacker/micropolis). The Super Nintendo is the only edition to offer [gifts](https://gamefaqs.gamespot.com/snes/588657-simcity/faqs/68878#section2), which increase land value when placed, allowing for higher zone density in the areas outside of the *downtown bonus*.

I have also experimented with optimizing for population by algorithm, and I am able to reach 700k by automatic code.

| city    | population | score | tax | crime | pollution | cash / year |
| ------- | ---------- | ----- | --- | ----- | --------- | ----------- |
| bigpop1 | 709,405    | 467.0 | 2%  | 140   | 174       | +2,206      |
| bigpop2 | 694,670    | 468.5 | 5%  | 175   | 179       | +7,944      |
| bigpop3 | 688,435    | 458.5 | 5%  | 218   | 171       | +7,568      |
| bigpop4 | 682,590    | 525.0 | 2%  | 116   | 168       | +470        |
| bigpop5 | 670,950    | 461.5 | 3%  | 219   | 171       | +4,194      |

## Citizens Demand More Housing

Score is calculated in two stages, the first uses seven penalty terms:

- crime, the citywide crime average
- pollution, the citywide pollution average
- housing, the citywide land value average
- taxes, the tax rate
- traffic, the citywide traffic average
- unemployment
- fire, the tiles burning at that moment

*Figure 2. The seven scoring terms. Six are bounded at zero and can only subtract, and traffic can be as high as 576. Unemployment has no lower bound, a negative value raises score.*

Six out of seven terms are bounded at zero. However, unemployment is unbounded:

```
unemployment = (resPop / (8 * jobs) - 1) * 255
```

When the number of jobs exceeds the residents zoned to fill them, **The Unemployment term goes negative and adds to the score**, providing a score bonus of about 128 points. No other term moves the score this much.

The second stage of score calculation is reduction by nine more penalties:

*Figure 3. Each penalty applies to what the one above it left, so the same factor is worth less further down the list. The right-hand column is what each costs a city scoring 940.*

The first three are penalties for failing to build a special zone. Surpassing 500 residents demands a stadium, 100 commercial an airport and 70 industrial a seaport. Where a stadium is placed does not matter to score, having no traffic or pollution. However, Airports and Seaports do reduce overall score, because of their high-density pollution and so they are not built.

## Citizens Demand Fountains

Each of those seven penalty terms is an average, and the [land value scan.cpp](https://github.com/SimHacker/MicropolisCore/blob/6811ae03b419949650e782331021cbb1f16a5486/packages/micropolis-engine/src/scan.cpp#L274) decides what is included by each 2x2 square of map by their tile ID. Grass, forest and park trees are under 64 and invisible to scoring, but **a fountain of ID 840 includes it for grading, even though nobody lives in it!**

These 2x2 squares otherwise carry no population, pulling down the crime penalty term by 33 points.

| tile id | tile                | land value | counted |
| ------- | ------------------- | ---------- | ------- |
| 0       | dirt                | no         | no      |
| ...     | ...                 | ...        | ...     |
| 2-20    | water               | +15        | no      |
| 21-43   | forest, park trees  | +15        | no      |
| 44-63   | rubble, flood, fire | no         | no      |
| 64-65   | bridge              | no         | yes     |
| 66-79   | road                | no         | yes     |
| ...     | ...                 | ...        | ...     |
| 840     | park fountain       | **no**     | **yes** |

*Figure 5. A 2x2 square is counted only when it contains tile ID 64 or greater, while land value is credited only from tile IDs below 44. The park building tool has a one in five chance to draw a fountain, ID 840. Placing fountains lower total crime penalty.*

## Park Overflow

Many guides recommend parks to reduce or absorb pollution, but they do not reduce pollution at all in [Micropolis](https://github.com/SimHacker/micropolis). However, pollution reduces land value, and land value drives crime, so parks lower crime indirectly by raising the land value around them.

Water, park and tree tiles all increase land value, and the engine counts them in 4x4 blocks. Every such tile credits its block 15 points, and the final credit to land value is this sum, plus downtown bonus, minus pollution.

However, this land value credit is kept as a single byte, and [smoothTerrain() in scan.cpp](https://github.com/SimHacker/MicropolisCore/blob/6811ae03b419949650e782331021cbb1f16a5486/packages/micropolis-engine/src/scan.cpp#L503) blends a block with its neighbors before it divides, so a block with its surroundings may sum *beyond 255 and wrap around*, counter-intuitively lowering land value.

*Figure 4. Number of Water, park or tree tiles in a 4x4 block against the land value it credits. Exactly 1/2 of the tiles (8) is the optimal value of greenery. At 9 of 16 tiles, it is in danger of driving down land value when summed with its neighbors.*

The [PC SimCity manual](https://archive.org/details/simcity_ibm_manual) declares that "Parks, like forests and water, raise the land value of surrounding zones". However, **too many parks or trees can push push down land value and increase crime.**

## Citizens Demand More Industries

Common strategy is to lay industrial zones by map edges, and this is recommended to maximize for population. The [SNES SimCity manual](https://archive.org/details/sim-city-usa) writes, "One of the great benefits of building industrial Zones along the edges is that half the pollution is off the map and doesn't affect the city. This is very important when it comes to heavily polluting industries, Airports and Coal Power Stations."

This is partly true. The [Micropolis](https://github.com/SimHacker/micropolis) engine spreads pollution outward from each source, divides the map into 4x4 blocks, and then creates an average of only the blocks that contain pollution:

```
pollutionAverage = ptot / pnum
```

- `ptot` is the pollution totaled over the dirty blocks
- `pnum` is the count of those blocks

Tiles that contain low levels of pollution help bring down total pollution by this averaging, a small high-scoring city should keep the light density pollution for itself. Models suggest that a city maximized for population should place industrial zones on map edges for other reasons, that the land value there is too low to for other kind of zones.

## Citizens Demand Humanity

Jay Wright Forrester's [Urban Dynamics](https://archive.org/details/urbandynamics0000forr) (1969) is often cited as an influence on Will Wright, Chief Designer of [SimCity (1989)](https://archive.org/details/msdos_SimCity_1989). *Urban Dynamics* is essentially a paperback simulation of cities. The [Micropolis](https://github.com/SimHacker/micropolis) simulation is vaguely recognizable as a minified version of the model in *Urban Dynamics* along with some small improvements in including spatial values of neighboring tiles that Forrester's models lack. Interestingly, both arrive at the same place: what improves a city most is *reducing* available housing:

> The concurrent reduction of excess housing is absolutely essential. It supplies the land for new job-creating structures. Equally important, the resulting housing shortage creates the population-stabilizing pressure that allows economic revival to proceed without being inundated by rising population.
>
> —Jay Forrester, [Counterintuitive Behavior of Social Systems](https://ocw.mit.edu/courses/15-988-system-dynamics-self-study-fall-1998-spring-1999/65cdf0faf132dec7ec75e91f9651b31f_behavior.pdf), 1971

[The Sumerian Game (1963)](https://en.wikipedia.org/wiki/The_Sumerian_Game), considered the antecedent of the city-building game genre, contains the same anti-pattern, where [I found the best strategy](https://www.jeffquast.com/post/hamurabi_bas/) is systemic starvation of 3% of the population. I think in 2026 it would be difficult to find a proponent for the housing and population shortages that all three of these models suggest optimizing towards.

[←](https://www.jeffquast.com/post/perfecting-terminal-character-width-using-correction-tables/)
