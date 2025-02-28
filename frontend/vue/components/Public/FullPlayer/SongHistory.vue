<template>
    <div id="station-history">
        <p v-if="history.length <= 0">
            {{ $gettext('No records to display.') }}
        </p>
        <div
            v-for="(row, index) in history"
            :key="row.sh_id"
            class="song"
        >
            <strong class="order">{{ history.length - index }}</strong>
            <img
                v-if="showAlbumArt"
                class="art"
                :src="row.song.art"
            >
            <div class="name">
                <strong v-html="row.song.title" />
                <span v-html="albumAndArtist(row.song)" />
            </div>
            <div class="break" />
            <small class="date-played text-muted">
                <span v-html="unixTimestampToDate(row.played_at)" />
            </small>
        </div>
    </div>
</template>

<script setup>
import {DateTime} from "luxon";

const props = defineProps({
    history: {
        type: Array,
        default: () => {
            return [];
        }
    },
    showAlbumArt: {
        type: Boolean,
        default: true
    },
});

const unixTimestampToDate = (timestamp) => {
    if (!timestamp) {
        return '';
    }

    return DateTime.fromSeconds(timestamp).toRelative();
};

const albumAndArtist = (song) => {
    return [song.artist, song.album].filter(str => !!str).join(' - ');
};
</script>

<style lang="scss">
#station-history {
    .song {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        width: 100%;
        line-height: normal;
        margin-bottom: 15px;

        &:last-child {
            margin-bottom: 0;
        }

        .order {
            display: flex;
            flex-direction: column;
            width: 35px;
            justify-content: center;
            margin-right: 5px;
            text-align: center;
        }

        .art {
            width: 40px;
            height: 40px;
            border-radius: 4px;
            margin-right: 5px;
        }

        .name {
            display: flex;
            flex: 1;
            flex-direction: column;
            justify-content: center;
        }

        .date-played {
            display: flex;
            flex-direction: column;
            justify-content: center;
            margin: 4px 0 0 40px;
        }

        .break {
            flex-basis: 100%;
            height: 0;
        }

        @media (min-width: 576px) {
            .date-played {
                margin-left: auto;
            }
            .break {
                display: none;
            }
        }
    }
}
</style>
