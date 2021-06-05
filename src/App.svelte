<script>
  let result = "(Click 'Generate More Names')";
  let names = "alpha\nbeta\ngamma\ndelta\nepsilon\nzeta\neta\ntheta\niota\nkappa\nla\nmu\nnu\nxi\nomicron\npi\nrho\nsigma\ntau\nupsilon\nphi\nchi\npsi\nomega\n";

  function generateNames()
  {
    var start = [];
    var next = [];
    for (var i=0; i<26; ++i) next["abcdefghijklmnopqrstuvwxyz"[i]] = [];

    var prev = "";
    var total_count = 0;
    var entries = 0;
    for (var i=0; i<names.length; ++i)
    {
      var ch = names[i];
      if (ch >= 'A' && ch <= 'Z') ch = String.fromCharCode( ch.charCodeAt(0) + 32 );

      if (ch >= 'a' && ch <= 'z')
      {
        if (prev == "")
        {
          ++entries;
          start.push( ch );
        }
        else
        {
          next[prev].push( ch );
        }
        prev = ch;
        ++total_count;
      }
      else
      {
        prev = "";
      }
    }

    var avg_count = (total_count / entries) | 0;
    var min_count = (avg_count / 2)   | 0;
    var max_count = (avg_count * 1.5) | 0;

    result = "";
    for (var i=0; i<10; ++i)
    {
      var n = ((Math.random() * (max_count - min_count)) + min_count) | 0;

      var prev = start[ (Math.random() * start.length)|0 ];
      result += prev;
      for (var j=1; j<n; ++j)
      {
        var set = next[ prev ];
        if (set && set.length > 0)
        {
          var ch = set[ (Math.random() * set.length)|0 ];
          result += ch;
          prev = ch;
        }
        else
        {
          break;
        }
      }
      result += "\n";
    }
  }
</script>

<main>
  <p><b>Generated Names</b><br>
    <textarea bind:value={result}></textarea>

  <p><button on:click={generateNames}>
    Generate More Names
  </button>

  <p><b>Training Set (Editable)</b><br>
  <textarea bind:value={names}></textarea>

  <p>
  v1.0 by Abe Pralle<br>
  <a href="https://github.com/AbePralle/NameGenerator">https://github.com/AbePralle/NameGenerator</a>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

  textarea { width: 300px; height: 250px; }

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
