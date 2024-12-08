<script>
  import { browser } from "$app/environment";
  import Slide from "../components/Slide.svelte";
  let activeIndex = 0;

  // Example slide data (mixed types)
  const slides = [
    {
      type: "component",
      props: {
        noteText:
          "A web person is someone who designs and/or develops websites.",
        content: `The value of the web person* has been undermined by services
            like Squarespace, Cargo, Wix, etc. We must recognize the differences between
            a template and a bespoke website, just like we do an Instagram story and a poster.`,
      },
    },
    {
      type: "component",
      props: {
        noteText:
          "Non-profits like the Internet Archive (Wayback Machine) are under attack from private interests.",
        content: `Websites are constantly decaying. We must advocate for their preservation*. Companies that choose to discontinue their services should invest in emulation.`,
      },
    },
    {
      type: "component",
      props: {
        content: `Websites are being threatened by the increasing dominance of social media. In order to fight for the right of individual expression, we must invest in our websites and the people behind them.`,
      },
    },
    {
      type: "component",
      props: {
        content: `We must embolden the role of the web designer. Oftentimes clients are reluctant to place website credits in their websites to maintain the illusion of in-house design. 
          <br /> <br />We should treat a web designer’s credit like a made-in-Italy label. `,
      },
    },
    {
      type: "component",
      props: {
        noteText: "View more about GDPR fines at gdpr.eu/fines/. ",
        content: `We must democratize access to critical information regarding web privacy and accessibility compliance. GDPR legislation*, if pursued equally, disproportionately punishes independent practices. Many beginning web persons are not aware of the legal ramifications of their design. Instead of punishing ignorance, invest more in giving independent practices access to the same tools as large corporations with legal teams.  `,
      },
    },
    {
      type: "component",
      props: {
        content: `Web persons need to share experience to further embolden their collective power. Unpaid internships are discouraged in favor of non-labor-based mentorships with younger designers. `,
      },
    },
    {
      type: "component",
      props: {
        content: `Websites are not just tools for conversion. The experience and aesthetic value of the website, even if inefficient for business, should be valued.   `,
      },
    },
    {
      type: "component",
      props: {
        content: `Content & programming by Oliver Buckley.
      <br /><br />
  Inspired by <a href='http://www--arc.com/' target='_blank'>www-arc.com</a>
      <br /><br />
   
  Completed for FIT GD-332
  
    `,
      },
    },
  ];

  //   flash click through opacity 0 opacity 100 every half a second

  if (browser) {
    const click = document.getElementById("click");
    setInterval(() => {
      click.style.opacity = 0;
      setTimeout(() => {
        click.style.opacity = 1;
      }, 250);
    }, 500);
  }

  function handleClick(e) {
    // Only increment activeIndex if the click is not on the span with id="de"
    if (e.target.id !== "de") {
      activeIndex = (activeIndex + 1) % slides.length;
    }
  }

  function closeSplash() {
    const splash = document.getElementById("splash");
    splash.style.display = "none";
  }
</script>

<div
  on:click={(e) => closeSplash()}
  id="splash"
  class="h-screen w-screen bg-[#FF0A0E] justify-center flex flex-col fixed top-0 left-0 z-[9999]"
>
  <h1 class="monument mx-auto text-center w-2/3">Web Design Manifesto</h1>
  <p
    id="click"
    class="monument !text-[38px] w-screen text-center fixed bottom-[40px]"
  >
    *click through*
  </p>
</div>

<div on:click={(e) => handleClick(e)}>
  {#if slides[activeIndex].type === "component"}
    <!-- Render using the Slide component -->
    {#key activeIndex}
      <Slide noteText={slides[activeIndex].props.noteText}>
        {@html slides[activeIndex].props.content}
      </Slide>
    {/key}
  {:else if slides[activeIndex].type === "html"}
    <!-- Render custom HTML content -->
    {@html slides[activeIndex].content}
  {/if}
</div>
