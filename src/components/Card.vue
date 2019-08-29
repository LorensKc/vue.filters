<template>
    <div class="card-inner">
        <div class="card-user">
            <div class="card-user-image">
                <img :src="card.author_pic" alt="card image">
            </div>
            <div class="card-user-info">
                <div class="cu-info-name">
                    @{{card.author_username}}
                </div>
                <div class="cu-info-link">
                    <a :href="card.link" target="_blank">{{card.social_network}}</a>
                </div>
            </div>
        </div>
        <div class="card-image">
            <img :src="card.pic" alt="author image">
        </div>
        <div class="card-info">
            <div class="card-info-item">
                <div class="ci-item-title">Likes</div>
                <div class="ci-item-count">{{card.likes_number}}</div>
            </div>
            <div class="card-info-item">
                <div class="ci-item-title">Comments</div>
                <div class="ci-item-count">{{card.comments_number}}</div>
            </div>
        </div>
        <div class="card-navigation">
            <button type="button" class="btn-approve" v-if="status === '' || status !== 'approve'" @click="changeStatus(card, 'approve')">Approve</button>
            <button type="button" class="btn-reject" v-if="status === '' || status !== 'reject'" @click="changeStatus(card, 'reject')">Reject</button>
            <button type="button" class="btn-restore" v-if="status" @click="changeStatus(card, '')">Restore</button>
        </div>
        <div class="current-status" v-if="status">
            {{'status → ' + status}}
        </div>
    </div>
</template>

<script>
export default {
    name: 'Card',
    props: ['card', 'status'],
    data(){
        return {

        }
    },
    methods: {
        changeStatus(card, status) {
            this.$emit('update-status', {card, status});
        }
    }
}
</script>

<style lang="scss">
    .card {
    width: calc(25% - ((20px * 4) / 3));
    position: relative;
    margin: 0 20px 20px 0;
    border: 1px solid #000;
        &:nth-child(4n + 4) {
            margin-right: 0;
        }
    }

    .card-inner {
        width: 100%;
        position: relative;
    }

    .card-user {
    width: 100%;
    padding: 15px;
    position: relative;
    display: flex;
    flex-flow: row nowrap;
    align-items: center;
    justify-content: flex-start;
    }

    .card-user-image {
    width: 50px;
    height: 50px;
    overflow: hidden;
    border-radius: 50%;
    background: #ccc;
    position: relative;

    img {
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        object-fit: cover;
        position: absolute;
    }
    }

    .card-user-info {
    width: calc(100% - 50px);
    position: relative;
    padding-left: 20px;
    }

    .cu-info-name,
    .cu-info-link {
    width: 100%;
    position: relative;
    font-size: 14px;
    }

    .cu-info-name {
    color: black;
    margin-bottom: 5px;
    }

    .cu-info-link {
    color: white;

    a {
        color: blue;
    }
    }

    .card-image {
    width: 100%;
    height: 0;
    overflow: hidden;
    position: relative;
    padding-bottom: 100%;
    background: #ccc;

    img {
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        object-fit: cover;
        position: absolute;
    }
    }

    .card-info {
    width: 100%;
    padding: 15px;
    position: relative;
    display: flex;
    flex-flow: row nowrap;
    align-items: center;
    justify-content: flex-start;
    }

    .card-info-item {
    width: 50%;
    position: relative;

    > div {
        width: 100%;
        position: relative;
    }
    }

    .ci-item-title {
    color: #aaa;
    font-size: 14px;
    margin-bottom: 3px;
    text-transform: uppercase;
    }

    .ci-item-count {
    font-size: 28px;
    font-weight: bolder;
    }
    
    .card-navigation {
        display: flex;
        flex-flow: row nowrap;
        align-items: center;
        justify-content: flex-start;
        border-top: 2px solid #e3e3e3;
        padding: 15px 20px;

        button {
            min-width: 110px;
            border: none;
            outline: none;
            padding: 7px 25px;
            background: transparent;
            font-size: 16px;
            color: white;
            cursor: pointer;
            margin: 0 5px;

            &:first-child {margin-left: 0;}
            &:last-child {margin-right: 0;}

            &.btn-reject {background: red;}
            &.btn-approve {background: green;}
            &.btn-restore {background: black;}
        }
    }

    .current-status {
        width: 100%;
        position: relative;
        padding: 10px 20px;
        font-size: 14px;
        font-style: italic;
        background: black;
        color: white;
    }
</style>

