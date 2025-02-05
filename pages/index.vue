<script setup lang="ts">
interface Speaker {
  name: string;
  title: string;
  description?: string;
  date: string;
  confirmed?: boolean;
  url?: string;
  linkedin?: string;
  img: string;
  expand: boolean;
}

let speakers: Speaker[] = reactive([
  {
    name: "Ayça Atabey",
    title: "Data protection, AI, and human rights issues",
    description: `This presentation will be a talk about a very interesting subject.`,
    date: "2023-02-01T17:45:00.000+01:00",
    url: "https://www.turing.ac.uk/people/enrichment-students/ayca-atabey",
    linkedin: "https://www.linkedin.com/in/ayca-atabey/",
    img: "https://www.turing.ac.uk/sites/default/files/styles/people/public/2022-08/ayca_atabey.jpg?itok=kyLTNTBb",
    expand: false,
  },
  {
    name: "Nicholas Boucher",
    title: "Invisible Hacks Against Machine Learning & Source Code",
    description:
      "Adversarially encoding text can cause lots of problems. In this talk, we will explore recent research into attacking real-world systems ranging from machine learning models to compilers by leveraging obscure text encodings.",
    date: "2023-04-30T17:45:00.000+01:00",
    confirmed: true,
    linkedin: "https://www.linkedin.com/in/bouchernicholas/",
    img: "https://www.cl.cam.ac.uk/~ndb40/assets/img/boucher.webp?h=8fa366f4b743a2c305532241cd0d30ea",
    expand: false,
  },
  {
    name: "Colin Ozanne",
    title: "Designing Effective Web Interfaces: Tips and Techniques",
    description: `In this talk, we will explore the key principles of design and how they can be applied to create visually appealing and intuitive web interfaces. We will also delve into techniques for building responsive websites that work well on a variety of devices, and discuss the importance of accessibility and performance in web design. We will cover practical tips and techniques for optimizing the usability of your website, including how to create clear and concise content, design effective calls to action, and conduct user testing. By the end of the talk, you will have a solid understanding of how to design and build web interfaces that are both visually appealing and user-friendly.`,
    date: "2023-02-05T05:00:00.000+01:00",
    url: "https://colinozanne.fr/",
    linkedin: "https://www.linkedin.com/in/colin-ozanne-99594822a/",
    img: "https://avatars.githubusercontent.com/u/71637999?v=4",
    expand: false,
  },
]).sort((a, b) => new Date(a.date).getTime() - new Date(b.date).getTime());

const parseDate = (date: string) => {
  const options = {
    weekday: "long",
    year: "numeric",
    month: "long",
    day: "numeric",
    hour: "numeric",
    minute: "2-digit",
    timeZone: "Europe/Paris",
  };
  return new Date(date).toLocaleDateString("en-GB", options);
};

let nextSpeaker = speakers.find(
  (speaker) => new Date(speaker.date).getTime() > Date.now()
);

const delay = ({ hours = 0, days = 0, date = Date.now() } = {}) => {
  let d = new Date(date);
  d.setHours(d.getHours() - hours);
  d.setDate(d.getDate() - days);
  return d;
};

const streamLink = "https://www.twitch.tv/krhacken";

onBeforeMount(() => {
  if (window.location.search.includes("test")) {
    let s = speakers.find((speaker) => speaker.name === "Colin Ozanne");
    if (s) s.name = "Colin Ozanne-Audroing";
  }
});
</script>

<template>
  <div class="page">
    <div class="head">
      <!--<p class="primary-text">
                Join us for <span class="gradient strong">conferences</span><br>
                and a <span class="gradient strong">quiz</span>!<br>
            </p>-->

      <div class="primary-text">
        <p class="left">
          <span>English</span>
          <span>Week</span>
        </p>
        <p class="right">
          <span class="gradient">2023</span>
        </p>
      </div>

      <p class="secondary-text">
        <span class="description">When & Where ?</span><br />
        <span class="date"
          >January 30 <span class="gradient">•</span> February 3 <br />
          <span class="gradient">Online</span></span
        >
      </p>
    </div>

    <!-- set to 1 week before first stream -->
    <div
      v-if="new Date() < delay({ days: 7, date: nextSpeaker.date })"
      class="stream"
    >
      <p class="next">
        Next talk will be from
        <span class="gradient strong">{{ nextSpeaker.name }}</span> on
        <span class="gradient strong">{{ parseDate(nextSpeaker.date) }}</span>
      </p>

      <iframe
        title="Test"
        src="https://tube.gnous.eu/videos/embed/0e6f37ea-8331-4e40-89b6-5d374c5150de?title=0&amp;warningTitle=0&amp;peertubeLink=0"
        allowfullscreen=""
        sandbox="allow-same-origin allow-scripts allow-popups"
        width="560"
        height="315"
        frameborder="0"
      ></iframe>
    </div>
    <div id="about" class="scroll-anchor"></div>
    <div class="intro">
      <span class="about-title"
        >What exactly is the <br /><span class="gradient strong"
          >english week ?</span
        ></span
      >
      <ol class="about-content">
        <li>
          The English Week is the perfect opportunity to improve your English
          skills and learn more about the world of IT.<br />
        </li>
        <li>
          Each year, the student committee of the computer science department
          organizes a week of conferences, a quiz, and encourages every teacher
          to teach their regular class in English.
        </li>
        <li>
          There is an incredible lineup of speakers who will be sharing their
          expertise on a variety of subjects, such as cybersecurity, web design,
          data protection, and even AI! These talks are not only a chance to
          deepen your understanding of these exciting topics, but also a
          fantastic opportunity to hone your English skills. Don't miss out on
          this amazing learning experience!
        </li>
        <hr class="separator-small" />
        <div class="small">
          The speakers at the English Week events are invited by the student
          committee of the computer science department and may not necessarily
          be affiliated with the school.
        </div>
      </ol>
    </div>

    <div class="speakers" id="speakers">
      <h2><span class="gradient">Talks</span></h2>
      <small>All times are shown in CET (Central European Time)</small>
      <small>Unconfirmed dates & times are marked with ~</small>
      <section class="speaker-container">
        <div
          class="speaker-card"
          v-for="speaker in speakers"
          :key="speaker.name"
        >
          <div class="card-container">
            <div class="user-image">
              <!-- Replace "/user.png" with the image of your which you want to show on your profile card-->
              <img class="speaker-img" :src="speaker.img" :alt="speaker.name" />
            </div>
            <h2 class="user-name">{{ speaker.name }}</h2>
            <p class="gradient strong" style="text-align: center">
              {{ speaker.confirmed ? "" : "~ " }} {{ parseDate(speaker.date) }}
            </p>
            <p class="about-user strong">
              {{ speaker.title }}
            </p>
            <span
              class="expand-control"
              @click="speaker.expand = !speaker.expand"
            >
              {{ speaker.expand ? "Hide" : "Show" }} details</span
            >

            <p v-if="speaker.expand" class="speaker-description">
              {{ speaker.description }}
            </p>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<style scoped lang="scss">
@import "assets/css/main.scss";

.scroll-anchor {
  visibility: hidden;
  position: relative;
  top: -150px;
}

.gradient {
  background: $gradient;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.strong {
  font-weight: 600;
}

div.page {
  padding-top: 3rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  max-width: 1920px;
  margin: 0 auto;

  div.head {
    width: 100%;
    display: flex;
    height: 60vh;
    flex-direction: row;
    justify-content: space-evenly;
    margin: 1rem 3rem 20rem;
    color: $white;

    @media screen and (max-width: 768px) {
      flex-direction: column;
      margin-top: 0;
    }

    .primary-text {
      display: flex;
      justify-content: flex-end;
      align-items: flex-end;
      gap: 1rem;
      align-self: center;
      max-width: 60rem;
      text-align: center;
      font-family: "Inter", sans-serif;
      font-weight: 600;
      font-size: 7.5rem;
      letter-spacing: 0.1rem;

      .left {
        display: flex;
        flex-direction: column;
        justify-content: flex-end;
        align-items: flex-end;
        font-size: inherit;
      }

      .right {
        display: flex;
        flex-direction: column;
        justify-content: flex-end;
        align-items: flex-start;
        font-size: inherit;
      }
    }

    .secondary-text {
      align-self: end;
      font-family: "Inter", sans-serif;
      font-size: 1rem;
      color: #ffffffb3;

      .description {
        display: inline-block;
        font-size: 1.5rem;
        font-weight: 600;
        margin-bottom: 0;
        text-align: left;
      }

      .date {
        display: inline-block;
        font-size: 2rem;
        font-weight: 600;
        margin-bottom: 1rem;
        text-align: left;
      }
    }

    p {
      width: 40%;
      font-size: 2.7rem;

      @media screen and (max-width: 768px) {
        width: 100%;
        font-size: 2rem;
      }

      span.blue {
        color: $blue;
      }

      span.red {
        color: $red;
      }

      small {
        font-size: 2.5rem;
      }
    }

    img {
      width: 15rem;
    }
  }

  div.stream {
    width: 100%;
    padding: 1rem 9rem;
    margin: 2rem 0 0;
    background-color: $black;
    text-align: center;
    line-height: 1.6rem;
    margin-bottom: 15rem;

    @media screen and (max-width: 768px) {
      padding: 1rem 3rem;
    }

    p.next {
      font-size: 1.5rem;
      font-family: "Inter", sans-serif;
      color: $white;
    }
  }

  div.intro {
    width: 100%;
    height: 70vh;
    padding: 1rem 9rem;
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    gap: 2rem;
    background-color: $black;
    color: $white;

    @media screen and (max-width: 768px) {
      padding: 1rem 2rem;
      flex-direction: column;
      gap: 0;
    }

    .about-title {
      display: block;
      font-size: 2.7rem;
      font-family: "Inter", sans-serif;
      color: $white;
      width: 100%;
    }

    .about-content {
      display: flex;
      flex-direction: column;
      list-style: none;
      counter-reset: li;

      li {
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: flex-start;
        margin-top: 1rem;
        margin-bottom: 1rem;
        counter-increment: li;
        font-size: 1.3rem;

        &::marker {
          visibility: hidden;
        }

        &::before {
          display: inline-block;
          content: counter(li);
          width: 4rem;
          background: linear-gradient(
            120deg,
            rgba(253, 29, 29, 1) 0%,
            rgb(69, 69, 252) 100%
          );
          -webkit-background-clip: text;
          -webkit-text-fill-color: transparent;
          font-size: 4rem;
          font-weight: 600;
          margin-right: 1.5rem;
        }

        &:first-child::before {
            margin-right: 0;
        }
      }

      .small {
        font-size: 1rem;
        color: #ffffffb3;
      }

      .separator-small {
        width: 100%;
        height: 1px;
        background-color: #ffffffb3;
      }
    }

    p {
      width: 100%;
      font-size: 1.4rem;
      text-align: justify;
    }
  }

  div.speakers {
    width: 85%;
    margin: 2rem 0 0;
    text-align: left;

    h2 {
      font-size: 3.5rem;
      font-family: "Inter", sans-serif;
      color: $white;
      margin-bottom: 1rem;
    }

    small {
      display: block;
      font-size: 1rem;
      margin-bottom: 1rem;
    }

    section {
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 4rem;
      margin: 6rem 6rem 0;

      .card-container {
        background-color: rgb(31, 31, 32);
        color: white;
        padding: 2rem;
        width: 25rem;
        height: 30rem;
        margin: 0.5rem;
        border-radius: 0.4rem;
        overflow: auto;
      }

      .user-image {
        position: relative;
        width: 100px;
        height: 100px;
        margin: 0 auto;
      }

      .user-image img {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        border-radius: 50%;
        object-fit: cover;
      }

      .user-name,
      .about-user {
        text-align: center;
        margin-top: 1.5rem;
      }

      .user-name {
        font-size: 1.4rem;
      }

      span.expand-control {
        display: block;
        margin-top: 1rem;
        font-size: 0.8rem;
        font-weight: 600;
        text-decoration: underline;
        text-align: center;
        cursor: pointer;
      }

      .speaker-description {
        text-align: justify;
      }
    }
  }
}
</style>
