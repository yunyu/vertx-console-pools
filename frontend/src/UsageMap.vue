<template>
    <div>
        <div class="usage-map">
            <div class="square" :class="usedSquareClass" v-for="n in used" :key="n" /><div class="square avail" v-for="o in avail" :key="used + o" /><div class="square empty" v-for="p in empty" :key="max + p" />
        </div>
        <div class="legend">
            <div class="square" :class="usedSquareClass"/> {{ used }} Used
            <div class="square avail"/> {{ avail }} Available
            <div class="queue">{{ queued }} Queued</div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        used: Number,
        max: Number,
        largest: Number,
        queued: Number
    },
    computed: {
        avail() {
            return this.max - this.used;
        },
        empty() {
            return this.largest - this.max;
        },
        ratio() {
            return this.used / this.max;
        },
        usedSquareClass() {
            if (this.ratio > 0.6) {
                return 'used-hot';
            } else if (this.ratio > 0.33) {
                return 'used-warm';
            } else {
                return 'used';
            }
        }
    }
}
</script>

<style lang="scss" scoped>
.usage-map {
    width: 100%;
    margin: 15px 0;
    line-height: 12px;
    display: block;
}

.square {
    width: 18px;
    height: 18px;
    display: inline-block;
    margin-right: 2px;
    transition: background 0.15s;

    @media (min-width: 1600px) {
        width: 22px;
        height: 22px;
    }
}

.used {
    background: #f9d67a;
}

.used-warm {
    background: #f39c3d;
}

.used-hot {
    background: #cd0000;
}

.avail {
    background: #bee1f4;
}

.empty {
    background: #f1f1f1;
}

.legend {
    height: 18px;
    line-height: 18px;

    .square {
        height: 10px;
        width: 10px;
        margin: 0;

        &.avail {
            margin-left: 8px;
        }
    }

    .queue {
        float: right;
        font-weight: bold;
    }
}
</style>