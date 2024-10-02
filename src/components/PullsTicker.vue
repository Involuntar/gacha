<template>
    <div>
        <p>У вас {{ pulls }} круток!</p>
        <button @click="pullCharacter">Крутить {{ guarant }}</button>
        <div style="display: flex; flex-direction: row; flex-wrap: wrap;">
        <p style="width:150px; hyphens: auto;" v-for="(pulledCharacter, index) in pulledCharacters" :key="index" class="pulled-character" :class="(isPressed ? 'pressed' : '')">
            <img :src="pulledCharacter.Icon" width="50" height="50"><br>{{ pulledCharacter.Name }}
        </p>
    </div>
    </div>
</template>

<script>

export default {
    props: {
        characters: Object,
        chances: Object
    },
    data() {
        return {
            pulls: 1,
            isPressed: false,
            pulledCharacter: undefined,
            rarities: [],
            pulledCharacters: [], 
            guarant: 0
        }
    },
    created() {
        setInterval(this.addPulls, 1000)
    },
    methods: {
        addPulls() {
            this.pulls++;
        },
        pullCharacter() {
            this.isPressed = true;
            if (this.pulls < 1) {
                this.pulledCharacter = "У Вас не хватает круток";
                return;
            }

            
            let sumChances = 0;
            for(let key in this.chances) {
                let starChances = this.chances[key]?.character + this.chances[key]?.weapon;
                if (isNaN(starChances)) {
                    starChances = this.chances[key];
                }
                sumChances += starChances;
            }

            console.log(sumChances);
            
            let fiveStarChar = this.chances["5"].character / sumChances;
            let fourStarChar = this.chances["4"].character / sumChances + fiveStarChar;
            let threeStar = this.chances["3"] / sumChances + fourStarChar;

            let fiveStarWeap = this.chances["5"].weapon / sumChances;
            let fourStarWeap = this.chances["4"].weapon / sumChances + fiveStarWeap;

            let sumFiveStarsChars = this.characters["5"].Characters.length;
            let sumFourStarsChars = this.characters["4"].Characters.length;
            let sumThreeStars = this.characters["3"].length;

            this.pulledCharacters = {};

            let weapOrChar = Math.floor(Math.random() * 2);
            if (weapOrChar === 1) {
                let charFourStar = Math.floor(Math.random() * this.characters["4"].Characters.length);
                let name = this.characters["4"].Characters[charFourStar].Name;
                let icon = this.characters["4"].Characters[charFourStar].Icon;
                        
                this.pulledCharacters[0] = {"Name": name + ": ⭐⭐⭐⭐", 
                    "Icon": icon};
            } else {
                let weapFourStar = Math.floor(Math.random() * this.characters["4"].Weapons.length);
                let name = this.characters["4"].Weapons[weapFourStar].Name;
                let icon = this.characters["4"].Weapons[weapFourStar].Icon;
                this.pulledCharacters[0] = {"Name": name + ": ⭐⭐⭐⭐", 
                "Icon": icon};
            }
            
            for (let i = 1; i < 10; i++) {
                let chance = Math.random();
                if (chance <= fiveStarChar + fiveStarWeap) { // Шанс выпадения 5*
                    let weapOrChar = Math.floor(Math.random() * 2);
                    let concreteItem = Math.random();
                    if (weapOrChar === 1) {
                        let name = this.characters["5"].Characters[Math.floor(concreteItem * sumFiveStarsChars)].Name;
                        let icon = this.characters["5"].Characters[Math.floor(concreteItem * sumFiveStarsChars)].Icon;

                        this.pulledCharacters[i] = {"Name": name + ": ⭐⭐⭐⭐⭐", 
                            "Icon": icon};
                        continue;
                    }
                    let name = this.characters["5"].Weapons[Math.floor(concreteItem * sumFiveStarsChars)].Name;
                    let icon = this.characters["5"].Weapons[Math.floor(concreteItem * sumFiveStarsChars)].Icon;

                    this.pulledCharacters[i] = {"Name": name + ": ⭐⭐⭐⭐⭐", 
                        "Icon": icon};
                }
                else if (chance <= fourStarChar + fourStarWeap) { // Шанс выпадения 4*
                    let weapOrChar = Math.floor(Math.random() * 2);
                    let concreteItem = Math.random();
                    if (weapOrChar === 1) {
                        let name = this.characters["4"].Characters[Math.floor(concreteItem * sumFourStarsChars)].Name;
                        let icon = this.characters["4"].Characters[Math.floor(concreteItem * sumFourStarsChars)].Icon;
                        
                        this.pulledCharacters[i] = {"Name": name + ": ⭐⭐⭐⭐", 
                            "Icon": icon};
                        continue;
                    }
                    let name = this.characters["4"].Weapons[Math.floor(concreteItem * sumFourStarsChars)].Name;
                    let icon = this.characters["4"].Weapons[Math.floor(concreteItem * sumFourStarsChars)].Icon;
                        
                    this.pulledCharacters[i] = {"Name": name + ": ⭐⭐⭐⭐", 
                        "Icon": icon};
                    
                }
                else if (chance <= threeStar) {
                let concreteItem = Math.random();
                this.pulledCharacters[i] = {"Name": this.characters["3"][Math.floor(concreteItem * sumThreeStars)].Name + ": ⭐⭐⭐", 
                    "Icon": this.characters["3"][Math.floor(concreteItem * sumThreeStars)].Icon
                }; // Шанс выпадения 3*
                }
            }
            this.pulls -= 1;
            this.guarant += 10;
        }
    }
}
</script>

<style>
.pulled-character {
    display: none;
}
.pressed {
    display: block;
}
</style>