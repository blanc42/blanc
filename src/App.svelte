<script>
  import { gsap } from "gsap";
  import { onMount } from "svelte";
  import ScrollTrigger from "gsap/ScrollTrigger";
import { Github, Linkedin } from 'lucide-svelte';

  let skills = [
    "Skill 1",
    "Skill 2",
    "Skill 3",
    "Skill 4",
    "Skill 5",
    "Skill 6",
    "Skill 7",
    "Skill 8",
    "Skill 9",
    "Skill 10",
    "Skill 11",
    "Skill 12",
    "Skill 13",
    "Skill 14",
    "Skill 15",
    "Skill 16",
    "Skill 17",
    "Skill 18",
    "Skill 19",
    "Skill 20",
    "Skill 21",
    "Skill 22",
    "Skill 23",
    "Skill 24",
    "Skill 25",
  ];

  let projects = [
    {
      title: "Project 1",
      description: "A innovative web application that revolutionizes user experience.",
      image: "https://picsum.photos/800/600?random=1",
      caseStudy: "#",
      github: "#",
    },
    {
      title: "Project 2",
      description: "An efficient mobile app for managing daily tasks and improving productivity.",
      image: "https://picsum.photos/800/600?random=2",
      caseStudy: "#",
      github: "#",
    },
    {
      title: "Project 3",
      description: "A cutting-edge machine learning model for predictive analytics in finance.",
      image: "https://picsum.photos/800/600?random=3",
      caseStudy: "#",
      github: "#",
    },
    {
      title: "Project 4",
      description: "An interactive data visualization tool for complex scientific datasets.",
      image: "https://picsum.photos/800/600?random=4",
      caseStudy: "#",
      github: "#",
    },
    {
      title: "Project 5",
      description: "A secure and scalable blockchain solution for supply chain management.",
      image: "https://picsum.photos/800/600?random=5",
      caseStudy: "#",
      github: "#",
    },
  ];

  let myText;
  let footer;

  function splitTextIntoSpans(element) {
    const text = element.innerText;
    const splitText = text
      .split("")
      .map((char) => `<span>${char}</span>`)
      .join("");
    element.innerHTML = splitText;
    return element.querySelectorAll("span");
  }

  onMount(() => {
    const chars = splitTextIntoSpans(myText);
    gsap.registerPlugin(ScrollTrigger);

    gsap.set(chars, { y: 50, opacity: 0 });

    gsap.to(chars, {
      duration: 0.5,
      y: 0,
      opacity: 1,
      stagger: 0.05,
      ease: "back.out(1.7)",
    });

    const pinnedSections = gsap.utils.toArray(".pinned");

    console.log(pinnedSections);
    const lastCard = document.querySelector(".scroll");

    pinnedSections.forEach((section, index, sections) => {
      let img = section.querySelector(".img");

      let nextSection = sections[index + 1] || lastCard;
      let endScalePoint = `top+=${nextSection.offsetTop - section.offsetTop} top`;

      gsap.to(section, {
        scrollTrigger: {
          trigger: section,
          start: "top top",
          end:
            // @ts-ignore
            index === sections.length
              ? `+=${lastCard.offsetHeight / 2}`
              : footer.offsetTop - window.innerHeight,
          pin: true,
          pinSpacing: false,
          scrub: 1,
        },
      });
      gsap.fromTo(
        img,
        { scale: 1 },
        {
          scale: 0.7,
          ease: "none",
          scrollTrigger: {
            trigger: section,
            start: "top top",
            end: endScalePoint,
            scrub: 1,
          },
        },
      );
      const heroH1 = document.querySelector(".hero");

      ScrollTrigger.create({
        trigger: document.body,
        start: "top top",
        end: "top+=400vh",
        scrub: 1,
        onUpdate: (self) => {
          let opacityProgress = self.progress;
          if (heroH1 instanceof HTMLElement) {
            heroH1.style.opacity = (1 - opacityProgress).toString();
          }
        },
      });
    });
  });
</script>

<main class="w-full h-full grid-background flex flex-col min-h-screen">
  <section
    class="hero pinned h-screen flex flex-col justify-center items-center py-10"
  >
    <div class="text-center flex flex-col items-center">
      <h1 bind:this={myText} class="text-[8rem] font-bold mb-2">__blanc__</h1>
      <h2 class="text-3xl">Frontend • Backend • DevOps</h2>
      <div class="icons flex gap-2 mt-4">
        <a href="https://github.com/blanc42" target="_blank">
          <Github class="w-8 h-8"/>
        </a>
        <a href="https://www.linkedin.com/in/blanc42/" target="_blank">
          <Linkedin class="w-8 h-8"/>
        </a>
      </div>
    </div>
  </section>

<section>
  <h2 class="text-6xl md:text-8xl font-bold mb-10 py-4 text-center">My Work</h2>
</section>

  {#each projects as project, index}
    <section
      class={`card pinned ${index === projects.length - 1 ? "scroll relative" : ""} w-screen h-screen flex justify-center items-center `}
    >
      <div class="img w-[1280px] h-[720px] px-4 md:px-0 relative group">
        <img
          class="w-full h-full object-cover"
          src={project.image}
          alt={project.title}
        />

        <div class="absolute h-48 bottom-0 w-full group-hover:bg-gradient-to-t group-hover:from-black/70 group-hover:via-black/40 group-hover:to-transparent transition-colors duration-200">
          <div class="absolute pt-16 inset-x-0 bottom-0 p-6 flex gap-2 flex-col justify-end text-white opacity-50 group-hover:opacity-100 transition-all duration-300 ease-in-out">
            <h3 class="text-xl md:text-2xl font-bold">{project.title}</h3>
            <h4 class="text-sm md:text-base">{project.description}</h4>
            <div class="flex gap-2">
              <a href={project.caseStudy} class="text-xs md:text-sm bg-white text-black px-3 py-1 rounded-full hover:bg-opacity-80 transition-all duration-300 ease-in-out">Case Study</a>
              <a href={project.github} class="text-xs md:text-sm bg-white text-black px-3 py-1 rounded-full hover:bg-opacity-80 transition-all duration-300 ease-in-out">GitHub</a>
            </div>
          </div>
        </div>


      </div>
    </section>
  {/each}

  <section
    bind:this={footer}
    class="footer w-full h-screen flex justify-center items-center"
  >
    <h1 class="text-8xl font-bold">footer</h1>
  </section>
  <!-- <div class="overflow-hidden whitespace-nowrap">
    <div class="animate-marquee">
      {#each skills as skill}
        <span class="mx-4 text-2xl">{skill}</span>
      {/each}
      {#each skills as skill}
        <span class="mx-4 text-2xl">{skill}</span>
      {/each}
    </div>
  </div> -->

  <!-- <section class="py-20">
    <div class="content-wrapper">
      <h2 class="text-6xl font-bold mb-10 py-10 text-center">My Work</h2>
      <div class="grid grid-cols-1 gap-8 px-6"> -->

  <!-- <div
            class="relative group overflow-hidden rounded-lg shadow-lg transition-all duration-300 ease-in-out h-full"
          >
            <div class="aspect-w-16 aspect-h-9 h-full">
              <img
                src={project.image}
                alt={project.title}
                class="w-full h-full object-cover"
              />
            </div>
            <div
              class="absolute inset-0 bg-black bg-opacity-50 group-hover:bg-opacity-70 transition-all duration-300 ease-in-out"
            >
              <div
                class="absolute inset-x-0 bottom-0 p-6 flex flex-col justify-end text-white opacity-50 group-hover:opacity-100 transition-all duration-300 ease-in-out"
              >
                <div
                  class="hidden md:block absolute inset-x-0 bottom-0 h-1/2 bg-gradient-to-t from-black to-transparent opacity-75"
                ></div>
                <div class="relative space-y-2 md:space-y-4">
                  <h3 class="text-xl md:text-2xl font-bold">{project.title}</h3>
                  <p class="text-sm md:text-base">
                    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed
                    do eiusmod tempor incididunt ut labore et dolore magna
                    aliqua.
                  </p>
                </div>
                <div
                  class="relative flex flex-col md:flex-row justify-between items-start md:items-center space-y-2 md:space-y-0 mt-4"
                >
                  <div class="space-x-2 md:space-x-4">
                    <a
                      href={project.caseStudy}
                      class="text-xs md:text-sm bg-white text-black px-3 py-1 rounded-full hover:bg-opacity-80 transition-all duration-300 ease-in-out"
                    >
                      <span class="hidden md:inline">Case Study</span>
                      <span class="md:hidden">📚</span>
                    </a>
                    <a
                      href={project.github}
                      class="text-xs md:text-sm bg-white text-black px-3 py-1 rounded-full hover:bg-opacity-80 transition-all duration-300 ease-in-out"
                    >
                      <span class="hidden md:inline">GitHub</span>
                      <span class="md:hidden">🐙</span>
                    </a>
                  </div>
                </div>
              </div>
            </div>
          </div> -->

  <!-- </div>
    </div>
  </section> -->
</main>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
  }

  main {
    width: 100%;
  }

  /* .animate-marquee {
    display: inline-flex;
    animation: marquee 60s linear infinite;
  }

  @keyframes marquee {
    0% {
      transform: translateX(0);
    }
    100% {
      transform: translateX(-50%);
    } */
  /* } */

  .grid-background {
    background-color: black;
    background-image: linear-gradient(
        to right,
        rgba(255, 255, 255, 0.1) 1px,
        transparent 1px
      ),
      linear-gradient(to bottom, rgba(255, 255, 255, 0.1) 1px, transparent 1px);
    background-size: 32px 32px;
  }

  /* .content-wrapper {
    max-width: 1200px;
    margin: 0 auto;
  } */
</style>
