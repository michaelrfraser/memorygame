<template>
    <b-container>
        <b-row v-for="row in rows" :key="row">
            <b-col v-for="card in rowItems[row-1]" :key="card.id" @click="onCardClicked(card)">
                <card class="mycard" :image="getIcon(card)" :flipped="card.flipped" :color="getColorStyle(card)"></card>
            </b-col>
        </b-row>
    </b-container>
</template>

<script>
    import Card from './Card.vue'
    import IconSet from '../data/icons'
    import Vue from 'vue'

    export default {
        name: 'CardTable',
        components: {
            'card': Card,
        },
        data() {
            return {
                cards: [],
                pick1: null,
                pick2: null,
                resetInterval: null,
            }
        },
        computed: {
            pairs() {
                return this.rows * this.columns / 2;
            },
            rowItems() {
                let rowobjs = [];
                for( var i = 0 ; i < this.rows ; ++i )
                {
                    let rowI = [];
                    for( var j = 0 ; j <  this.columns ; ++j )
                        rowI.push( this.cards[i*this.columns+j]);
                    rowobjs.push( rowI );
                }
                return rowobjs;
            }

        },
        props: {
            rows: Number,
            columns: Number,
        },
        created() {
            for( var i = 0 ; i < this.pairs ; ++i )
            {
                Vue.set( this.cards, i*2, {
                    id: i*2,
                    type: i,
                    flipped: false,
                });
                Vue.set( this.cards, i*2+1, {
                    id: i*2+1,
                    type: i,
                    flipped: false,
                });
            }
            this.shuffle();
        },
        methods: {
            getIcon( card ) {
                return IconSet.icons[card.type].icon;
            },
            getColorStyle( card )
            {
                return IconSet.icons[card.type].color;
            },
            resetFlippedCards()
            {
                this.pick1.flipped = false;
                this.pick2.flipped = false;
                this.pick1 = null;
                this.pick2 = null;
                clearInterval( this.resetInterval );
                this.resetInterval = null;
            },
            onCardClicked( card ) {
                if( this.resetInterval )
                {
                    this.resetFlippedCards();
                }

                if( !card.flipped )
                {
                    if( this.pick1 == null )
                    {
                        card.flipped = true;
                        this.pick1 = card;
                    }
                    else if( this.pick2 == null )
                    {
                        card.flipped = true;
                        this.pick2 = card;

                        if( this.pick1.type != this.pick2.type )
                        {
                            this.resetInterval = setInterval( () => {
                                this.resetFlippedCards();
                            }, 2000 );
                        }
                        else
                        {
                            this.pick1 = null;
                            this.pick2 = null;
                        }
                    }

                   
                }
            },
            shuffle() {
                let currentIndex = this.cards.length;
                while( 0 !== currentIndex )
                {
                    let randomIndex = Math.floor( Math.random() * currentIndex );
                    --currentIndex;

                    let temp = this.cards[currentIndex];
                    Vue.set( this.cards, currentIndex, this.cards[randomIndex] );
                    Vue.set( this.cards, randomIndex, temp );
                }
            }
        }
    }
</script>

<style>
    
</style>