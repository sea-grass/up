<script>
    import { onMount } from "svelte";
    import gsap from "gsap";

    let won = false;
    let level = 0;

    let goalRef;
    let upTween, rightTween, downTween, leftTween;

    const forward = (tween) => tween.timeScale(1).play();
    const reverse = (tween) => tween.timeScale(0.5).reverse();
    const reset = (tween) => tween.seek(0).pause();

    // up
    const moveUp = () => forward(upTween);
    const reverseUp = () => reverse(upTween);
    // right
    const moveRight = () => forward(rightTween);
    const reverseRight = () => reverse(rightTween);
    // down
    const moveDown = () => forward(downTween);
    const reverseDown = () => reverse(downTween);
    // left
    const moveLeft = () => forward(leftTween);
    const reverseLeft = () => reverse(leftTween);

    const resetPositions = () => {
        [upTween,rightTween,downTween,leftTween].forEach(reset);
    }

    onMount(() => {
        gsap.to(goalRef, { x: 300 - 32 });
        gsap.to(".up.box,.left.box,.down.box,.right.box", {
            x: 300 - 32,
            y: 300 - 32,
        });

        // up
        upTween = gsap.timeline({
            onComplete: () => {
                won = true;
                // gsap.to(".win", {
                //     x: window.innerWidth / 2,
                //     y: window.innerHeight / 2,
                // });
                // upTween.pause();
            },
        });
        upTween.to(".up.box", { y: 0, duration: 3 });
        upTween.pause();
        // right
        rightTween = gsap.timeline();
        rightTween.to(".right.box", { x: 600 - 32, duration: 3 });
        rightTween.pause();
        // down
        downTween = gsap.timeline();
        downTween.to(".down.box", { y: 600 - 32, duration: 3 });
        downTween.pause();
        // left
        leftTween = gsap.timeline();
        leftTween.to(".left.box", { x: 0, duration: 3 });
        leftTween.pause();
    });
</script>

<svelte:head>
    <title>Up - Can you go up?</title>
</svelte:head>

<h1>Up</h1>
<p>A point and drag game</p>
<div class="chrome">
    <div class="header">
        Level {level}
    </div>
    <div class="left-sidebar">
        <p><i>Up</i> is a game best played with a desktop mouse.</p>
        <p>There is a collection of boxes in the center of the playing area.</p>
        <p>Your goal is to move to box labelled "up" into the goal area.</p>
    </div>
    <!-- <div class="right-sidebar"></div>
    <div class="footer"></div> -->
    <div class="canvas">
        <div class="up box" on:mouseenter={moveUp} on:mouseleave={reverseUp}>
            up
        </div>
        <div
            class="right box"
            on:mouseenter={moveRight}
            on:mouseleave={reverseRight}
        >
            right
        </div>
        <div class="down box" on:mouseenter={moveDown} on:mouseleave={reverseDown}>
            down
        </div>
        <div class="left box" on:mouseenter={moveLeft} on:mouseleave={reverseLeft}>
            left
        </div>
        <div class="goal box" bind:this={goalRef}>goal</div>
    </div>
</div>

{#if won}
<div class="win">
    <div>You win!</div>
    <button class="play-again" on:click={e => {
        resetPositions();
        won = false;
    }}>Play again?</button>
</div>
{/if}

<style>
    * {
        box-sizing: border-box;
    }
    .chrome {
        display: grid;
        grid-template-columns: 300px 1fr 300px;
        grid-template-areas: 
            "l h r"
            "l c r"
            "f f f";
    }
    .chrome .header {
        grid-area: h;
        /* border: 1px solid black; */
        margin: 0 auto;
        width: 600px;
        padding: 16px;
    }
    .chrome .left-sidebar {
        grid-area: l;
    }

    .chrome .right-sidebar {
        grid-area: r;
    }

    .chrome .footer {
        grid-area: f;
    }

    .chrome .canvas {
        grid-area: c;
        border: 1px solid black;
    }

    .box {
        display: inline-block;
        width: 64px;
        height: 32px;
        color: white;
        position: absolute;
        text-align: center;
    }

    .up.box {
        background: blue;
    }

    .right.box {
        background: rgb(145, 145, 100);
    }

    .down.box {
        background: green;
    }

    .left.box {
        background: red;
    }

    .goal.box {
        background: none;
        border: 1px dotted black;
        color: black;
        z-index: -1;
    }

    .win {
        display: inline-block;
        position: absolute;
        max-height: 320px;
        background: rgb(145, 145, 100);
        padding: 32px;
        color: white;
        font-size: 32px;
    }

    .canvas {
        position: relative;
        margin: 0 auto;
        width: 600px;
        height: 600px;
        border: 1px solid black;
    }
</style>
