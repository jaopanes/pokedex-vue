<template>
    <div class="card">
        <div class="image-card">
            <figure>
                <img :src="currentImg" alt="Placeholder image" />
            </figure>
        </div>

        <div class="content-card">
            <h2 class="title-card">{{ num }} - {{ name | upper }}</h2>
            <p class="subtitle-card">
                Tipo <strong>{{ pokemon.type }}</strong>
            </p>

            <button @click="mudarSprite">
                Mudar sprite
            </button>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    created: function () {
        axios.get(this.url).then((res) => {
            this.pokemon.type = res.data.types[0].type.name;
            this.pokemon.front = res.data.sprites.front_default;
            this.pokemon.back = res.data.sprites.back_default;

            this.currentImg = this.pokemon.front;
        });
    },

    data() {
        return {
            isFront: true,
            currentImg: "",

            pokemon: {
                type: "",
                front: "",
                back: "",
            },
        };
    },

    props: {
        num: Number,
        name: String,
        url: String,
    },

    filters: {
        upper: function (value) {
            return value[0].toUpperCase() + value.slice(1);
        },
    },

    methods: {
        mudarSprite: function () {
            if (this.isFront) {
                this.isFront = false;
                this.currentImg = this.pokemon.back;
            } else {
                this.isFront = true;
                this.currentImg = this.pokemon.front;
            }
        },
    },
};
</script>

<style>
.card {
    width: 100%;
    background-color: #fff;
    border-top: 10px solid #e23149;
    color: #16204c;
}

.card .image-card {
    background-color: #f4f6fb;
    display: flex;
    justify-content: center;
}

.card .title-card {
    font-size: 18px;
}

.card .subtitle-card {
    margin-top: 5px;
    font-size: 13px;
}

.card .content-card {
    padding: 20px;
}

.card .subtitle-card {
    text-transform: capitalize;
}

.card button {
    height: 40px;
    margin-top: 20px;
    padding: 0 10px;
    background: #424e6d;
    color: #FFF;
    font-weight: 600;
}
</style>