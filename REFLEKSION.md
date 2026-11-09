# Refleksion – Figma til kode

**Gruppemedlemmer:** Simon og Oskar

## Vores proces

Vi tog udgangspunkt i lærerens Astro-skabelon. I starten havde vi svært ved at finde rundt i sider, layouts og komponenter. Vi brugte undervisningsmateriale, Google, hjælp fra klassekammerater og AI til at forstå, hvordan delene hang sammen.

Efter feedbacken arbejdede vi videre i en kopi af vores projekt og rettede én fil ad gangen. Vi tilføjede blandt andet login-panelet, doughnut-diagrammerne og fotoet ved “Our vision”. Vi rettede også navigationen og gjorde alle medarbejdernes profilsider tilgængelige.

## Eksempel 1 – Astro og medarbejdersider

I den første aflevering var Sarah den eneste medarbejder med en profilside. I `getStaticPaths()` havde vi filtreret listen, så Astro kun fik Sarahs data til at bygge en side. Vi fjernede filteret og brugte samme profilskabelon til alle medarbejdere:

js

return team.map((employee) => ({
params: { slug: employee.slug },
props: { employee },
}));

`params` bestemmer medarbejderens adresse, og `props` giver siden de data, den skal vise. Vi rettede også links på Team og About. Bagefter åbnede vi alle medarbejdersiderne og kontrollerede, at de virkede.

## Eksempel 2 – Login og CSS-fallback

I den første aflevering manglede login-panelet. I `Login.astro` bruger vi nu Popover API. Attributten `popovertarget="login-panel"` forbinder knappen med panelets id, så browseren kan åbne det.

På brede skærme bruger vi Anchor Positioning til at placere panelet ved Login-knappen. Den placering ligger i en `@supports`-regel, som tjekker, om browseren understøtter de nødvendige CSS-funktioner.

En CSS-fallback er den grundløsning, browseren kan bruge, hvis den nyere CSS ikke understøttes. I vores projekt kan en browser med Popover API stadig åbne panelet, selvom Anchor Positioning mangler. Panelet vises så midt på skærmen. Vi har også valgt den centrerede placering på smalle skærme.

Vi åbnede panelet, brugte Tab til at flytte fokus mellem felterne og knappen og lukkede det med både Escape og krydset. Vi gjorde også vinduet smallere og kontrollerede, at panelet stadig kunne bruges. Login fungerer som en demo.

## Eksempel 3 – Subgrid og layout

Vi bruger subgrid på About og caseartiklen, så sektionerne kan følge sidens fælles kolonner. På About kan baggrunden ved Core Values fylde hele bredden, mens kortene placeres i sidens indre kolonner.

På caseartiklen bruger vi denne regel:

css
@supports (grid-template-columns: subgrid) {
.case-article {
grid-template-columns: subgrid;
}
}

Artiklen spænder over sidens grid og bruger kolonnerne derfra. Uden subgrid bruger den sit almindelige grid. Vi gennemgik About ved forskellige vinduesbredder. Det viste, hvordan siden tilpassede sig.

## Andre ændringer i layout og funktioner

I Financial Projections bruger vi container queries til at ændre kortenes bredde efter pladsen i selve komponenten. På en smal container kan kortene fylde 88 % af kortområdet. Grundbredden er `min(280px, 100%)`. I første aflevering var pilene deaktiverede. Vi tilføjede `scrollBy()`, så man kan bladre frem og tilbage.

FAQ’en bruger `details` og `summary` til at åbne og lukke spørgsmålene. Vi har kontrolleret, at spørgsmålene kan åbnes og lukkes. CSS tilføjer animation med `interpolate-size`, når browseren understøtter de nødvendige funktioner. Ellers åbner svaret direkte. Animationen og fallbacken er endnu ikke testet særskilt.

Vi bruger også Relative Color Syntax på login-knappens hover- og fokusfarve. Farven beregnes ud fra knappens grundfarve, og der er en fast farve som fallback.

I Core Values stod “Read More” i forskellig højde, fordi beskrivelserne havde forskellig længde. Vi brugte lodret flexbox og `margin-top: auto` til at placere linkene nederst i kortene og kontrollerede rettelsen i browseren.

## API, billeder og CSS

Vi bruger lærerens API til blandt andet medarbejdere, FAQ og caseindhold. API’et giver også billedadresser til medarbejderportrætterne og casebillederne. Fotoet ved “Our vision” ligger i projektets egen billedmappe. Hvis et API-kald fejler under build, kan koden bruge de gemte data og billeder som backup.

`tokens.css` samler genbrugelige værdier som farver og afstande. `global.css` indeholder fælles styling, blandt andet `--flow-space`, som styrer afstanden mellem tekstelementer i caseartiklen. Styling, der kun bruges af én komponent, ligger i dens Astro-fil. Et eksempel på Defensive CSS er `overflow-wrap: anywhere` på kortene, så lange ord kan brydes, når der er lidt plads.

## Test og browserstatus

Vi gennemgik hele sitet manuelt. Vi testede blandt andet medarbejdersider, login, tastaturfokus, FAQ, forsidelinks og layoutet ved forskellige vinduesbredder. `npm run build` gennemførte med 11 sider og beskeden `Complete!`.

## Brug af AI

Vi brugte ChatGPT og Codex til at forstå kode, gennemgå feedback og få forslag til rettelser.

## Kilder

- [MDN – Anchor Positioning og browserunderstøttelse](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/position-anchor)
- [MDN – Container queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_containment/Container_queries)
- [MDN – Subgrid](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Grid_layout/Subgrid)
- [MDN – interpolate-size og browserunderstøttelse](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/interpolate-size)
