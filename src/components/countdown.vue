<template>
    <div>
        <div class="countdown" v-if="days > 0 || hours > 0 || minutes > 0 || seconds > 0">
            {{ days }}d {{ hours | padStart }}h {{ minutes | padStart }}m {{ seconds | padStart }}s
        </div>
        <div class="active" v-else>
            Ativo
        </div>
    </div>
</template>

<script lang="ts">
    import { Prop, Component, Vue } from 'vue-property-decorator';

    @Component({
        filters: {
            padStart(value: any) {
                return value.toString().padStart(2, '0');
            },
        },
    })
    export default class Countdown extends Vue {
        @Prop({ default: '' })
        public begin?: string;

        @Prop({ default: '' })
        public end?: string;

        public now: number = Math.trunc((new Date()).getTime() / 1000);

        get beginTimestamps() {
            return this.begin
                ? Math.trunc(Date.parse(this.begin) / 1000)
                : this.now;
        }

        get seconds() {
            return (this.beginTimestamps - this.now) % 60;
        }

        get minutes() {
            return Math.trunc((this.beginTimestamps - this.now) / 60) % 60;
        }

        get hours() {
            return Math.trunc((this.beginTimestamps - this.now) / 60 / 60) % 24;
        }

        get days() {
            return Math.trunc((this.beginTimestamps - this.now) / 60 / 60 / 24);
        }

        get endTimestamps() {
            return this.end
                ? Math.trunc(Date.parse(this.end) / 1000)
                : 0;
        }

        mounted() {
            window.setInterval(() => {
                this.now = Math.trunc((new Date()).getTime() / 1000);
                if(this.endTimestamps === this.now) this.$emit('finished');
                if(this.beginTimestamps === this.now) this.$emit('begin');
            }, 1000);

            if(this.endTimestamps <= this.now) this.$emit('finished');
        }
    }
</script>