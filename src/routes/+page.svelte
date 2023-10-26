<head>
    <title>
        Omikuji
    </title>
</head>
<script>
    import Icon from '@iconify/svelte';
    import {onMount} from 'svelte';

    onMount(() => {
        const btn = document.querySelector('button.choose');
        const audioBtn = document.querySelector('button.audio');
        const bgAudio = document.querySelector("audio");
        const omikujiBox = document.querySelector('#omikujiBox');
        const omikujiPaper = document.querySelector('#omikujiPaper');
        const now = new Date();
        //Wait for the page to load
        document.onreadystatechange = function () {
            if (document.readyState !== "complete") {
                document.querySelector(
                    ".all").classList.add('hidden')
                document.querySelector(
                    "#loader").classList.add('opacity-100')
            } else {
                document.querySelector(
                    "#loader").classList.add('opacity-0')
                document.querySelector(
                    ".all").classList.add('opacity-100')
            }
        };
        //Check if the user has pulled today
        if (localStorage.getItem('lastPull') !== JSON.stringify(now.getDate())) {
            localStorage.setItem('hasPulled',"0");
        }
        //Audio player
/*        setTimeout(() => {
            bgAudio.play();
        },700)*/
        bgAudio.volume = 0.15;
        //Randomize
        let randomBlessing = () => {
            let blessing = ['Curse', 'ModestB', 'SmallB', 'FutureB', 'Blessing', 'GreatB'];
            return chooseRandom(blessing);
        }
        //Hiding the warning box if the user clicks anywhere
        window.addEventListener('click', () => {
            const hide = document.querySelector('.hide');
            hide.classList.add('hidden');
        })
        //Choose a corresponding blessing from randomized number
        let chooseRandom = (a) => {
            let x = Math.floor((Math.random() * 100 + 1));
            if (x <= 10) {
                return a[0];
            } else if (x <= 30) {
                return a[1];
            } else if (x <= 50) {
                return a[2];
            } else if (x <= 70) {
                return a[3];
            } else if (x <= 90) {
                return a[4];
            } else {
                return a[5];
            }
        }
        //Randomize the Omikuji on User load
        const random = randomBlessing();
        let randomOnLoad = () => {
            let x = Math.floor((Math.random() * 4 + 1));
            omikujiContent = `Omikuji_${random}${x}.png`;
        }
        randomOnLoad();
        //Maintaining data if user has pulled today
            if (localStorage.getItem('hasPulled') === "1") {
                omikujiContent = localStorage.getItem('omikuji');
                aruContent = localStorage.getItem('aru');
                setTimeout(() => {
                    btn.classList.add('off');
                }, 30)
                omikujiBox.classList.add('active');
                setTimeout(() => {
                    omikujiPaper.classList.add('active');
                }, 1200)
            }
        //Audio button
        audioBtn.addEventListener('click',() => {
            const audio = document.querySelector('audio');
            if (audio.paused) {
                audio.play();
                buttonIcon = 'charm:sound-down';
            } else {
                audio.pause();
                buttonIcon = 'charm:sound-mute';
            }
        })
        //Omikuji Animation
        let omikujiAnimation = () => {
            setTimeout(() => {
                omikujiPaper.classList.add('active');
            }, 1700)
            setTimeout(() => {
                omikujiBox.classList.add('opacity-0');
            }, 2650)
        }
        //Pulling the Omikuji
        btn.addEventListener('click', () => {
            const d = new Date();
            localStorage.setItem('lastPull', JSON.stringify(d.getDate()))
            let x = Math.floor((Math.random() * 4 + 1));
            omikujiBox.classList.add('active');
            btn.classList.add('off');
            btn.classList.remove('hover:bg-yellow-700');
            /*setTimeout(() => {
                omikujiContent = `Omikuji_${random}${x}.png`;
            }, 1500)*/
            omikujiAnimation();
            setTimeout(() => {
                aruContent = `Aru_${random}.png`;
                //btn.innerHTML = 'Come back Daily!'
            }, 1900)
            setTimeout(() => {
                localStorage.setItem('omikuji',omikujiContent);
                localStorage.setItem('hasPulled',"1");
                localStorage.setItem('aru',aruContent);
            }, 1901)
        })
    });
    let omikujiContent;
    let aruContent = "Aru.png";
    let buttonIcon = 'charm:sound-mute';
</script>

<div class="all">
<!--    <div class="absolute top-0 right-0 left-0 bottom-0 self-center" id="loader"></div>-->
    <div class="fixed left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2 z-20 hide">
        <div class="flex justify-center items-center">
            <div class="flex-col flex float justify-center items-center gap-2 w-72 h-44 p-6 bg-gray-800 font-bold text-white text-center border-4 rounded-xl">
                <div class="text-xl">Scroll/Zoom out to match your display!</div>
                <Icon icon="ic:sharp-swipe" class="h-3/4 w-3/4"/>
                <audio src="/Theme_97.ogg" hidden loop></audio>
            </div>
        </div>
    </div>
    <main class="relative w-full min-w-[1920px] h-[1080px] max-h-[1080px] overflow-hidden select-none">
        <div class="overflow-hidden background">
            <div class="relative w-[1920px] h-full">
                <div class="flex absolute bg-gray-800/50 rounded-full top-3 left-3 p-2">
                    <button class="audio">
                        <Icon icon="{buttonIcon}" class="w-[50px] h-[50px] text-white"/>
                    </button>
                </div>
                <div class="grid grid-cols-2 items-center justify-center gap-2 px-7">
                    <div class="relative w-[700px] ml-40 float">
                        <img src={aruContent} alt="Aru" class="nodrag w-full h-full object-contain">
                    </div>
                    <div class="flex flex-col justify-center items-center h-full">
                        <div class="absolute z-10 top-20 w-[500px]">
                            <img src="Banner.png" class="nodrag w-full h-full object-contain" alt="Banner">
                        </div>
                        <div class="absolute w-[1000px]">
                            <img src="/Omikuji_Shop.png" alt="Omikuji" class="nodrag w-full h-full object-contain">
                        </div>
                        <div>
                            <div class="relative">
                                <div class="relative flex justify-center z-10 h-[450px] duration-1000" id="omikujiBox">
                                    <img src='/Omikuji_box.png' alt="Omikuji" class="nodrag w-full h-full object-contain"/>
                                </div>
                            </div>
                            <div class="flex absolute z-10 w-[890px] bottom-[350px] right-[50px] duration-1000" id="omikujiPaper">
                                <img src={omikujiContent} alt="Omikuji" class="nodrag w-full h-full"/>
                            </div>
                        </div>
                        <div class="h-16 flex justify-center relative top-20 z-20 w-[520px] ">
                            <button class="choose brightness-[1.2] text-white font-bold py-2 px-4 rounded-full transition duration-300">
                                <img src="/Button1.png" alt="button">
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </main>
</div>

<style>
    #loader {
        border: 12px solid #f3f3f3;
        border-radius: 50%;
        border-top: 12px solid #444444;
        width: 70px;
        height: 70px;
        animation: spin 1s linear infinite;
    }
    @keyframes spin {
        100% {
            transform: rotate(360deg);
        }
    }
    .background {
        background-image: url("/BG_NewYearFestival.png");
        background-position: top;
        background-size: cover;
        background-repeat: no-repeat;
    }

    .hidden {
        display: none;
    }
    :global(.off) {
        pointer-events: none;
        cursor: not-allowed;
        filter: grayscale(100%);
    }
    @keyframes float {
        0% {
            transform: translateY(0px);
        }
        50% {
            transform: translateY(10px);
        }
        100% {
            transform: translateY(0px);
        }
    }

    :global(body) {
        background-color: black;
    }

    :global(#omikujiBox) {
        position: relative;
        transform: translateY(0);
        opacity: 1;
    }

    :global(#omikujiBox.active) {
        transform: translateY(100px);
        opacity: 0;
    }

    :global(#omikujiPaper.active) {
/*        position: relative;*/
        transform: translateY(0);
        opacity: 1;
    }

    :global(#omikujiPaper) {
        transform: translateY(100px);
        opacity: 0;
    }

    .float {
        animation: float 3s ease-in-out infinite;
    }

    .container {
        overflow-x: auto;
        white-space: nowrap;
    }
</style>
