<script>
    import "$lib/style/vars.css";
    import { onMount } from "svelte";

    const targetTimestamp = new Date('2024-06-15T21:59:59').getTime();

    let countdowns = [];

    function getTimeRemaining() {
        const now = new Date().getTime();
        const difference = targetTimestamp - now;

        if (difference <= 0) {
            return "Time's up!";
        }

        const seconds = Math.floor((difference / 1000) % 60);
        const minutes = Math.floor((difference / 1000 / 60) % 60);
        const hours = Math.floor((difference / (1000 * 60 * 60)) % 24);
        const days = Math.floor(difference / (1000 * 60 * 60 * 24));

        return `${days}d ${hours}h ${minutes}m ${seconds}s`;
    }

    function addCountdown() {
        try {
        const countdown = getTimeRemaining();
        countdowns = [...countdowns, countdown];

        // Remove the countdown after the animation (5s)
        setTimeout(() => {
            countdowns = countdowns.filter((c) => c !== countdown);
        }, 5000);
    } catch {}
    }

    onMount(() => {
        const interval = setInterval(addCountdown, 1000);
        return () => clearInterval(interval);
    });
</script>

<svelte:head>
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
        href="https://fonts.googleapis.com/css2?family=Jacquard+12&family=Pixelify+Sans:wght@400..700&display=swap"
        rel="stylesheet"
    />
    <meta property="og:title" content="Blockcoin" />
    <meta property="og:type" content="website" />
    <meta property="og:url" content="https://blockcoin.social" />
    <meta property="og:image" content="https://blockcoin.social/assets/logo.png" />
    <meta property="og:description" content="Blockcoin is a new social media built around a fictionnal currency that you can earn by posting!" />
    <meta name="theme-color" content="#BB01FF" />
    <meta name="description" content="Blockcoin is a new social media built around a fictionnal currency that you can earn by posting!" />
    <title>Blockcoin</title>
</svelte:head>

<div id="blur" />
<div class="elipse" id="e-1" />
<div class="elipse" id="e-2" />
<div class="background" />

{#each countdowns as time (time)}
    <div class="countdown">{time}</div>
{/each}

<style>
    .countdown {
        font-family: "Pixelify Sans", sans-serif;
        font-weight: 400;
        font-style: normal;
        color: white;
        font-size: var(--text-size-6);
        animation: moveUp 5s linear forwards;
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        text-align: center;
    }

    @keyframes moveUp {
        0% {
            transform: translateY(0);
        }
        100% {
            transform: translateY(-100vh);
        }
    }

    .background {
        background: var(--background-data);
        width: 100%;
        height: 100%;
        position: fixed;
        top: 0;
        left: 0;
        z-index: -7;
    }
    #blur {
        backdrop-filter: blur(var(--background-blur));
        z-index: -5;
        position: fixed;
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
    }

    .elipse {
        border-radius: 458px;
        width: 458px;
        height: 458px;
        z-index: -6;
        display: var(--background-elipse-display);
        position: fixed;
    }

    #e-1 {
        background: var(--background-elipse-1-color);
        top: 0;
        left: 0;
    }

    #e-2 {
        background: var(--background-elipse-2-color);
        bottom: 0;
        right: 0;
    }
    @media screen and (max-width: 1270px) {
        #blur {
            height: 100%;
        }
        .elipse {
            height: 56vw;
            width: 56vw;
        }
    }
    @media screen and (max-width: 425px) {
        .elipse {
            height: 76vw;
            width: 100%;
        }
        #e-1 {
            top: 0;
            left: 0;
        }

        #e-2 {
            bottom: 0;
            right: 0;
        }
    }
</style>
