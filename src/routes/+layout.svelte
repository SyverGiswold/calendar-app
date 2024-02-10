<style>
  :root {
    --bw-black: #0D0D0D;
    --bw-white: #F1F1F1;

    --Primary-Colors-primary: #323338;
    --Primary-Colors-secondary: #40424A;
    --Primary-Colors-white-60: hsla(0, 0%, 95%, 0.6);

    --Color-Sat-green: #0F9D58;
    --Color-Sat-red: #DA4437;
    --Color-Sat-yellow: #F4B400;
    --Color-Sat-blue: #4285F4;

    --gap: 5px;
    --border-radius: 20px;

    color: var(--bw-white);

    

    font-family: open sans, sans-serif;
    font-size: 150%; /* 24px equivalent */

    --h1: 1.6667rem;
    --h2: 1rem;
  }

  :global(body) {
    background-color: var(--Primary-Colors-primary);
  }

  :global(h1,h2,h3,h4,h5,h6) {
    font-weight: 400;
  }
</style>

<slot /> 