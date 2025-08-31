<template>
    <div class="card">
        <section class="info">
            <img src="@/assets/image-2.jpg" 
                alt="" 
                @load="load_img_group.push(true)">
            <div class="text">
                <h1>{{ text }}</h1>
                <div v-if="end_flag_blink"></div>
            </div>
            <div class="text">
            <h1>{{ text_2 }}</h1>
            <div v-if="!end_flag_blink"></div>
            </div>
        </section>
        <section class="links">
            <a href="https://github.com/SerezhenkoAM">
                <SiGithub />
            </a>
            <a href="https://t.me/zero_zero_eight">
                <SiTelegram />
            </a>
            <a href="https://codepen.io/gang_bang_love_sex_bomba">
                <SiCodepen />
            </a>
        </section>
        <section class="stack">
            <h2>Stack development</h2>
            <div class="table">              
                <IoLogoVue />                
                <IoLogoReact />         
                <IoLogoJavascript />
                <BiLogoPython />
                <IoLogoSass />                
                <IoLogoFigma />
                <BiLogoPostgresql />
            </div>
        </section>
        <section class="works">
            <h2>My projects</h2>
            <Carousel>
                <Slide v-for="slide in works" :key="slide">
                <img :src=slide 
                    alt="" 
                    class="work-item"
                    @load="load_img_group.push(true)" 
                >
                </Slide>
                <template #addons>
                    <Navigation style="cursor: none;"/>
                    <Pagination />
                </template>
            </Carousel>
        </section>
        <section class="active_job">
            <h2>Active job</h2>
            <ul>
                <li>Website landing</li>
                <li>The store's website</li>
                <li>Telegram Mini Apps</li>
                <li>Configuration server for website</li>
            </ul>
        </section>
        <footer>
            <p>Made in SERG® Studio</p>
            <Logo :height="50" :width="50" :activate_logo="true" class="logo" />
        </footer>
    </div>
</template>
<script>
import 'primeicons/primeicons.css'
import { IoLogoVue, IoLogoReact, IoLogoJavascript, IoLogoSass, IoLogoFigma } from 'vue-icons-plus/io';
import 'vue3-carousel/carousel.css'
import { BiLogoPostgresql, BiLogoPython } from 'vue-icons-plus/bi'
import { Carousel, Slide, Pagination, Navigation } from 'vue3-carousel'
import Logo from './Logo.vue';
import { SiCodepen, SiGithub, SiTelegram } from 'vue-icons-plus/si'
export default {
    name: 'own-card',
    components: {
        IoLogoVue,
        IoLogoReact,
        IoLogoJavascript,
        IoLogoSass,
        IoLogoFigma,
        BiLogoPython,
        BiLogoPostgresql,
        SiCodepen, SiGithub, SiTelegram,
        Carousel, Slide, Pagination, Navigation,
        Logo
    },
    data() {
        return {
            works: [
            require('@/assets/work-1.png'),
            require('@/assets/work-2.png'), 
            require('@/assets/work-3.png')
        ],
        text: '',
        text_2: '',
        end_flag_blink: true,
        load_img_group: []
        }
    },
    methods: {
        async typeText(text, time, perem) {
            this[perem] = '';
            for (const char of text) {
                this[perem] += char;
                await this.delay(time);
            }
            },
            delay(ms) {
                return new Promise(resolve => setTimeout(resolve, ms));
            },
        typeWriter() {
            setTimeout(() => {
                this.typeText('ALEXEY SEREZHENKO',100, 'text') 
            },1000)
            setTimeout(() => {
                this.end_flag_blink = false
                this.typeText('UI/UX Designer | Web Developer',100, 'text_2') 
            },3000)
        }
    },
    watch: {
        load_img_group: {
            handler(val) {
            if(val.length === 4) {
                this.$emit('mounted', true)
                this.typeWriter()
            }
        }, deep: true}
    },
}
</script>
<style lang="less" scoped>

@keyframes fade {
    from {
        opacity: 0;
        transform: translateY(-10px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

@keyframes blink {
    from {
        opacity: 0;
    } to {
        opacity: 1;
    }
}



.card {
    background: rgba(255,255,255,0.1);
    border-radius: 25px;
    backdrop-filter: blur(4px);
    border: 2px solid rgba(255,255,255,0.2);
    padding: 10px;
    min-width: 300px;
    max-width: 500px;
    margin: 0 auto;
    animation: fade 1s ease-in-out;
}

h1, h2 {
    color: white;
    font-family: 'Roboto';
    text-align: center;
}


.info {
    & > img {
        margin-top: 1rem;
        height: 250px;
        width: 250px;
        border-radius: 50%;
        border: 2px solid rgba(255,255,255,0.2);
        margin: 0 auto;
        display: block;
    }
    & > h1 {
        margin-top: 0;
    }
}

.text {
    display: flex;
    justify-content: center;
    align-items: end;
    margin: 1rem 0;
                  & > h1{
                  margin: 0;
              }
    & > div{
            background-color: black;
              height: 2px;
              width: 15px;
              border-radius: 25px;
              animation: blink .8s ease-in-out;
              animation-iteration-count: infinite;
              margin-left: 5px;
        }
}

.links {
    display: flex;
    justify-content: space-around;
    margin-top: 2rem;
    border-top: 2px solid rgba(255,255,255,0.2);
    border-bottom: 2px solid rgba(255,255,255,0.2);
    padding: 1rem 0 1rem 0;
    & > a {
        color: white;
        & > i {
            font-size: 2rem;
        }
        & > svg {
            width: 2rem;
            height: 2rem;
        }
    }
}

.stack {
    & > {
        color: white;
    }
    border-bottom: 2px solid rgba(255,255,255,0.2);
    margin-top: 2rem;
    & > .table {
        display: grid;
        grid-template-columns: repeat(4,1fr);
        grid-template-rows: repeat(2,1fr);
        justify-items: center;
        align-items: center;
        & > svg{
            height: 3rem;
            width: 100%;
            margin: 1rem;
        }
    }
}

.works {
    & > div {
    padding: 10px;
    }
}

.work-item {
        width: 100%;
        height: 100%;
        border-radius: 25px;
        }

.active_job {
    border-top: 2px solid rgba(255,255,255,0.2);
    margin-top: 2rem;
    ul > li {
        color: white;
        font-family: 'Roboto';
        font-size: 1.1rem;
        margin: 1rem 0;
        font-weight: bold;
        list-style-type: hiragana;
    }
}

footer {
    border-top: 2px solid rgba(255,255,255,0.2);
    margin-top: 2rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-family: 'Roboto';
    color: white;
    font-size: 1rem;
    padding: 10px;
}

* {
    cursor: none !important;
}


@media (max-width:600px) {
    .card {
        width: 90vw;
    }
    h1 {
        font-size: 1.2rem;
    }
    h2 {
        font-size: 1.1rem;
    }
    .info {
        & > img {
            height: 200px;
            width: 200px;
        }
    }
    .links {
        & > a {
            & > i {
                font-size: 2rem;
            }
        }
    }
    .stack {
    & > {
        color: white;
    }
    border-bottom: 2px solid rgba(255,255,255,0.2);
    margin-top: 2rem;
    & > .table {
        display: grid;
        grid-template-columns: repeat(4,1fr);
        grid-template-rows: repeat(2,1fr);
        justify-items: center;
        align-items: center;
        & > svg{
            height: 2.5rem;
            width: 100%;
            margin: 1rem;
        }
    }
}
    }
</style>