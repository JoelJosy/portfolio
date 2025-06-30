<script lang="ts">
	import gsap from "gsap";

    let counter1: HTMLDivElement;
    let counter2: HTMLDivElement;
    let counter3: HTMLDivElement;

    interface Props {
        onComplete?: () => void;
    }

    let { onComplete = () => {} }: Props = $props();

    $effect(() => {
        if (counter3) {
            for (let i = 0; i < 2; i++) {
                for (let j = 0; j < 10; j++) {
                    const div = document.createElement('div');
                    div.className = "num";
                    div.textContent = j.toString();
                    counter3.appendChild(div);
                }
            }

            const finalDiv = document.createElement('div');
            finalDiv.className = "num";
            finalDiv.textContent = "0";
            counter3.appendChild(finalDiv);
        }

        function animate(counter: Element, duration: number, delay: number = 0) {
            const numHeight = counter.querySelector('.num')?.clientHeight || 0;
            const totalDistance = (counter.querySelectorAll('.num').length - 1) * numHeight;

            gsap.to(counter, {
                y: -totalDistance,
                duration: duration,
                delay: delay,
                ease: "power2.inOut",
            });
        }

        // Start animations
        if (counter3) animate(counter3, 4);
        if (counter2) animate(counter2, 4.5);
        if (counter1) animate(counter1, 1.5, 3);

        gsap.to(".digit", {
            top: "-150px",
            stagger: {
                amount: 0.2,
            },
            delay: 4.5,
            duration: 0.8,
            ease: "power4.inOut",
        });

        gsap.from(".loader-1", {
            width: 0,
            duration: 4.5,
            ease: "power2.inOut",
        });

        gsap.from(".loader-2", {
            width: 0,
            delay: 1.5,
            duration: 1.5,
            ease: "power2.inOut",
        });

        gsap.to(".loader", {
            background: "none",
            delay: 4.5,
            duration: 0.1,
            ease: "power2.inOut",
        });

        gsap.to(".loader-1", {
            x: 75,
            y: 75,
            delay: 4.5,
            duration: 0.5,
        });

        gsap.to(".loader-2", {
            rotate: 90,
            // x: 75,
            y: -50,
            delay: 4.5,
            duration: 0.5,
        });

        gsap.to(".loader", {
            scale: 40,
            duration: 1,
            delay: 5.5,
            ease: "power2.inOut",
        });

        gsap.to(".loader", {
            rotate: -40,
            x: -1800,
            y: 1000,
            duration: 1,
            delay: 5.5,
            ease: "power2.inOut",
        });

        gsap.to(".loading-screen", {
            opacity: 0,
            duration: 0.5,
            delay: 6,
            ease: "power1.inOut",
            onComplete: onComplete
        });
    });
</script>

<div class="loading-screen">
    <div class="loader">
        <div class="loader-1 bar"></div>
        <div class="loader-2 bar"></div>
    </div>
    <div class="counter">
        <div bind:this={counter1} class="counter-1 digit">
            <div class="num">0</div>
            <div class="num">1</div>
        </div>
        <div bind:this={counter2} class="counter-2 digit">
            <div class="num">0</div>
            <div class="num">1</div>
            <div class="num">2</div>
            <div class="num">3</div>
            <div class="num">4</div>
            <div class="num">5</div>
            <div class="num">6</div>
            <div class="num">7</div>
            <div class="num">8</div>
            <div class="num">9</div>
            <div class="num">0</div>
        </div>
        <div bind:this={counter3} class="counter-3 digit">
            <div class="num">0</div>
            <div class="num">1</div>
            <div class="num">2</div>
            <div class="num">3</div>
            <div class="num">4</div>
            <div class="num">5</div>
            <div class="num">6</div>
            <div class="num">7</div>
            <div class="num">8</div>
            <div class="num">9</div>
        </div>
    </div>
</div>

<style>
    .loading-screen {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: #000;
        color: #fff;
        pointer-events: none;
        font-family: 'inter', sans-serif;
    }
    .counter {
        position: fixed;
        left: 50px;
        bottom: 50px;
        display: flex;
        height: 102px;
        font-size: 100px;
        line-height: 102px;
        clip-path: polygon(0 0, 100% 0, 100% 102px, 0 102px);
        font-weight: 400;
        overflow: hidden;
    }

    .counter-1,
    .counter-2,
    .counter-3 {
        position: relative;
        top: 0px;
    }
    
    .loader {
        position: absolute;
        top: 50%;
        left: 50%;
        width: 300px;
        height: 50px;
        transform: translate(-50%, -50%);
        display: flex;
        background: rgb(80, 80, 80);
    }

    .loader-1 {
        position: relative;
        background: #fff;
        width: 100px;
    }

    .loader-2 {
        position: relative;
        background: #fff;
        width: 200px;
    }
    .bar {
        height: 50px;
    }
</style>
