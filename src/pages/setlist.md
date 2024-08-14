---
layout: ../layouts/Layout.astro
title: DMB Setlist Generator
---

<div>
  <pre id="setlist"></pre>
  <hr />
  <form>
    <table>
      <tr>
        <td><label>Setlist length</label></td>
        <td><input type="range" min="5" max="25" id="setlistLength" value="20"/></td>
      </tr>
      <tr>
        <td><label>Encore length</label></td>
        <td><input type="range" min="1" max="4" id="encoreLength" value="3"/></td>
      </tr>
      <tr>
        <td><label>Teases</label></td>
        <td><input type="checkbox" id="teases" /></td>
      </tr>
      <tr>
        <td><label>Partials</label></td>
        <td><input type="checkbox" id="partials" /></td>
      </tr>
      <tr>
        <td><label>Segues</label></td>
        <td><input type="checkbox" id="segues" /></td>
      </tr>
      <tr>
        <td colspan="2">
          <a id="refresh">Refresh</a>
          <a id="copy">Copy</a>
        </td>
      </tr>
    </table>
  </form>
</div>

<script>
const songs = {
  "Remember Two Things": [
    "Recently",
    "One Sweet World",
    "The Song That Jane Likes",
    "Minarets",
    "Seek Up",
    "I'll Back You Up",
    "Christmas Song",
  ],

  "Under the Table and Dreaming": [
    "The Best of What's Around",
    "What Would You Say",
    "Satellite",
    "Rhyme & Reason",
    "Typical Situation",
    "Dancing Nancies",
    "Ants Marching",
    "Lover Lay Down",
    "Jimi Thing",
    "Warehouse",
    "Pay for What You Get",
    "#34",
  ],

  Crash: [
    "So Much to Say",
    "Two Step",
    "Crash Into Me",
    "Too Much",
    "#41",
    "Say Goodbye",
    "Drive In Drive Out",
    "Let You Down",
    "Lie in Our Graves",
    "Cry Freedom",
    "Tripping Billies",
    "Proudest Monkey",
  ],

  "Before These Crowded Streets": [
    "Pantala Naga Pampa",
    "Rapunzel",
    "The Last Stop",
    "Don't Drink the Water",
    "Stay (Wasting Time)",
    "Halloween",
    "The Stone",
    "Crush",
    "The Dreaming Tree",
    "Pig",
    "Spoon",
  ],

  Everyday: [
    "I Did It",
    "When the World Ends",
    "The Space Between",
    "Dreams of Our Fathers",
    "So Right",
    "If I Had It All",
    "What You Are",
    "Angel",
    "Fool to Think",
    "Sleep to Dream Her",
    "Mother Father",
    "Everyday",
  ],

  "Busted Stuff": [
    "Busted Stuff",
    "Grey Street",
    "Where Are You Going",
    "You Never Know",
    "Captain",
    "Raven",
    "Grace Is Gone",
    "Kit Kat Jam",
    "Digging a Ditch",
    "Big Eyed Fish",
    "Bartender",
  ],

  "Some Devil": [
    "Dodo",
    "So Damn Lucky",
    "Gravedigger",
    "Some Devil",
    "Trouble",
    "Grey Blue Eyes",
    "Save Me",
    "Stay or Leave",
    "An' Another Thing",
    "Oh",
    "Baby",
    "Up and Away",
    "Too High",
  ],

  "Stand Up": [
    "Dreamgirl",
    "Old Dirt Hill (Bring That Beat Back)",
    "Stand Up (For It)",
    "American Baby Intro",
    "American Baby",
    "Smooth Rider",
    "Everybody Wake Up (Our Finest Hour Arrives)",
    "Out of My Hands",
    "Hello Again",
    "Louisiana Bayou",
    "Stolen Away on 55th & 3rd",
    "You Might Die Trying",
    "Steady As We Go",
    "Hunger for the Great Light",
  ],

  "Big Whiskey & the GrooGrux King": [
    "Grux",
    "Shake Me Like a Monkey",
    "Funny the Way It Is",
    "Lying in the Hands of God",
    "Why I Am",
    "Dive In",
    "Spaceman",
    "Squirm",
    "Alligator Pie",
    "Seven",
    "Time Bomb",
    "Baby Blue",
    "You & Me",
  ],

  "Away from the World": [
    "Broken Things",
    "Belly Belly Nice",
    "Mercy",
    "Gaucho",
    "Sweet",
    "The Riff",
    "Belly Full",
    "If Only",
    "Rooftop",
    "Snow Outside",
    "Drunken Soldier",
  ],

  "Come Tomorrow": [
    "Samurai Cop (Oh Joy Begin)",
    "Can't Stop",
    "Here On Out",
    "That Girl Is You",
    "She",
    "Idea of You",
    "Virginia in the Rain",
    "Again and Again",
    "bkdkdkdd",
    "Black and Blue Bird",
    "Come On Come On",
    "Do You Remember",
    "Come Tomorrow",
    "When I'm Weary",
  ],

  "Walk Around the Moon": [
    "Walk Around the Moon",
    "Madman's Eyes",
    "Looking for a Vein",
    "The Ocean and the Butterfly",
    "It Could Happen",
    "Something to Tell My Baby",
    "After Everything",
    "All You Wanted Was Tomorrow",
    "The Only Thing",
    "Break Free",
    "Monsters",
    "Singing From the Windows",
  ],

  Unreleased: [
    "#40",
    "Bismarck",
    "Blackjack",
    "Blue Water",
    "Break for It",
    "Cha Cha",
    "Cigarette Lit",
    "Crazy Easy",
    "Death on the High Seas",
    "Deed Is Done",
    "A Dream So Real",
    "Dreamed I Killed God",
    "Falling Off the Roof",
    "Get in Line",
    "Good Good Time",
    "Heathcliff's Haiku Warriors",
    "Improv/Jam",
    "JTR",
    "Kill the King",
    "Light Lift Me Up",
    "Little Thing",
    "Loving Wings",
    "Monkey Man",
    "Once on a Wild Afternoon",
    "People People",
    "Plastic Girl",
    "Shotgun",
    "Sister",
    "Spotlight",
    "Straight Shot",
    "Sugar Will",
    "Sweet Up and Down",
  ],

  Misc: [
    "#27",
    "Anyone Seen The Bridge",
    "Beach Ball",
    "Corn Bread",
    "Eh Hee",
    "Granny",
    "Help Myself",
    "Joyride",
    "Kill the Preacher",
    "Little Red Bird",
    "New Song",
    "Trouble With You",
    "Water Into Wine",
    "What Will Become of Me",
    "Write a Song",
  ],
};

function generateSetlist(setlistLength, encoreLength, extras) {
  const { teases, partials, segues } = extras;
  const setlist = [];

  const albums = Object.keys(songs);
  while (setlist.length != setlistLength) {
    const album = albums[Math.floor(Math.random() * albums.length)];
    const albumSongs = songs[album];
    const song = albumSongs[Math.floor(Math.random() * albumSongs.length)];

    if (setlist.some((s) => s === song)) continue;

    const tease = teases && Math.random() < 0.03 ? " [tease]" : "";
    const partial = partials && !tease && Math.random() < 0.01 ? " [partial]" : "";
    const segue = segues && Math.random() < 0.03 ? " »" : "";
    setlist.push(`${song}${tease}${partial}${segue}`);
  }

  return [
    setlist.slice(0, setlist.length - encoreLength).join("\n"),
    setlist.slice(-encoreLength).join("\n"),
  ].join("\n------------------------\n");
}

function update() {
  document.getElementById("setlist").innerHTML = generateSetlist(
    document.getElementById("setlistLength").value,
    document.getElementById("encoreLength").value,
    {
      teases: document.getElementById("teases").checked,
      partials: document.getElementById("partials").checked,
      segues: document.getElementById("segues").checked,
    },
  );
}

async function copy() {
  const text = document.getElementById("setlist").innerHTML;
  try {
    await navigator.clipboard.writeText(text);
  } catch (error) {
    console.log("error", error);
  }
  alert("Copied to clipboard");
}

update();
document.getElementById("refresh").addEventListener("click", update);
Array.from(document.getElementsByTagName("input"))
  .forEach(() => addEventListener("change", update));
document.getElementById("copy").addEventListener("click", copy);
</script>
