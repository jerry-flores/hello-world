# hello-world
class Monster():
    def __init__(self, name, hp=20):
        self.exp = 0
        self.name = name
        self.type = "Normal"
        self.max_hp = hp
        self.current_hp = hp
        self.attacks = {'wait': 0}
        self.possible_attacks = {'sneak_attack': 1, 'slash': 2, 'ice_storm': 3, 'fire_storm': 3, 'whirlwind': 3, 'earthquake': 2, 'double_hit': 4, 'tornado': 4,       'wait': 0}

    def add_attack(self, attack_name):
        no_attacks = len(self.attacks)
        if attack_name in self.attacks.keys():
            return False
        if attack_name not in self.possible_attacks.keys():
            return False
        if no_attacks < 4:
            self.attacks[attack_name] = self.possible_attacks[attack_name]
        elif no_attacks == 4:
            mini = min(self.attacks.values())
            l = sorted(self.attacks.keys())
            d = {}
            for i in l:
                d[i] = self.possible_attacks[i]
            self.attacks = d
            for k,v in self.attacks.items():
                if v == mini:
                    self.attacks.pop(k)
                    break
            self.attacks[attack_name] = self.possible_attacks[attack_name]
        return True
    def remove_attack(self, attack_name):
        if attack_name in self.attacks.keys():
            self.attacks.pop(attack_name)
            if len(self.attacks) == 0:
                self.attacks['wait'] = 0
                return True
        elif attack_name not in self.attacks.keys() or attack_name not in self.possible_attacks.keys():
            return False
    def win_fight(self):
        self.exp +=5
        self.current_hp = self.max_hp
    def lose_fight(self):
        self.exp += 1
        self.current_hp = self.max_hpv    
    def sorting(self):
        return sorted(self.attacks.items(), key = lambda i : (-i[1],i[0]))


def monster_fight(monster1, monster2):
    mon1Attacks = Monster.sorting(monster1)
    mon2Attacks = Monster.sorting(monster2)
    if (monster1.attacks == {"wait":0}) and (monster2.attacks == {"wait":0}):
        return -1, None, None
    i = 0
    j = 0
    round1 = 1
    mon1AttacksUsed = []
    mon2AttacksUsed = []
    while True:
        round1 += 1
        monster2.current_hp -= mon1Attacks[i][1]
        mon1AttacksUsed.append(mon1Attacks[i][0])
        if monster2.current_hp <= 0: 
            monster2.lose_fight()
            monster1.win_fight()
            return round1, monster1, mon1AttacksUsed
        
        monster1.current_hp -= mon2Attacks[j][-1]
        mon2AttacksUsed.append(mon2Attacks[j][0])
        if monster1.current_hp <= 0: 
            monster1.lose_fight()
            monster2.win_fight()
            return round1, monster2, mon2AttacksUsed
        
        i += 1
        j += 1
        if i > len(mon1Attacks): i = 0
        if j > len(mon2Attacks): j = 0
if __name__=="__main__":
    a = Monster("a", 9)
    b = Monster("b", 9)
    a.add_attack("ice_storm")
    b.add_attack("ice_storm")
    a.remove_attack("wait")
    b.remove_attack("wait")
    round1, winner, moves = monster_fight(a, b)








