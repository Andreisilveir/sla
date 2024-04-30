import random


class Personagem:
    def __init__(self, nome, vida, ataque, defesa):
        self.nome = nome
        self.vida = vida
        self.ataque = ataque
        self.defesa = defesa
        self.level = 1
        self.exp = 0
        self.exp_para_level_up = 100

    def atacar(self, alvo):
        dano = self.ataque - alvo.defesa
        if dano > 0:
            alvo.vida -= dano
            print(f"{self.nome} atacou {alvo.nome} e causou {dano} pontos de dano.")
        else:
            print(f"{self.nome} atacou {alvo.nome}, mas não causou nenhum dano.")

    def verificar_vida(self):
        if self.vida <= 0:
            print(f"{self.nome} foi derrotado!")
            return True
        else:
            return False

    def ganhar_experiencia(self, exp):
        self.exp += exp
        print(f"{self.nome} ganhou {exp} pontos de experiência!")
        if self.exp >= self.exp_para_level_up:
            self.level_up()

    def level_up(self):
        self.level += 1
        self.exp -= self.exp_para_level_up
        self.exp_para_level_up = int(self.exp_para_level_up * 1.5)
        print(f"{self.nome} subiu para o nível {self.level}!")

    def __str__(self):
        return f"{self.nome} (Nível {self.level}) - Vida: {self.vida} - Ataque: {self.ataque} - Defesa: {self.defesa}"


class Jogador(Personagem):
    def __init__(self, nome, vida, ataque, defesa):
        super().__init__(nome, vida, ataque, defesa)
        self.itens = []

    def usar_item(self, item):
        if item in self.itens:
            self.itens.remove(item)
            item.usar(self)
            print(f"{self.nome} usou {item.nome}.")
        else:
            print(f"{self.nome} não possui esse item.")

    def __str__(self):
        return f"Jogador: {super().__str__()}"


class Monstro(Personagem):
    def __init__(self, nome, vida, ataque, defesa, exp_recompensa):
        super().__init__(nome, vida, ataque, defesa)
        self.exp_recompensa = exp_recompensa

    def __str__(self):
        return f"Monstro: {super().__str__()}"


class Item:
    def __init__(self, nome, efeito):
        self.nome = nome
        self.efeito = efeito

    def usar(self, personagem):
        personagem.vida += self.efeito
        if personagem.vida < 0:
            personagem.vida = 0
        print(f"{personagem.nome} recuperou {abs(self.efeito)} pontos de vida.")


def batalha(jogador, monstro):
    print(f"Batalha entre {jogador.nome} e {monstro.nome}!")
    while True:
        print(jogador)
        print(monstro)
        jogador.atacar(monstro)
        if monstro.verificar_vida():
            jogador.ganhar_experiencia(monstro.exp_recompensa)
            break
        monstro.atacar(jogador)
        if jogador.verificar_vida():
            break


# Criando o jogador
jogador = Jogador("Herói", 100, 20, 10)

# Criando os monstros
monstro1 = Monstro("Dragão", 200, 30, 5, 50)
monstro2 = Monstro("Esqueleto", 50, 10, 2, 20)

# Criando itens
item1 = Item("Poção de Vida", 30)
item2 = Item("Poção de Força", 10)

# Adicionando itens ao jogador
jogador.itens.append(item1)
jogador.itens.append(item2)

# Iniciando a batalha
batalha(jogador, monstro1)

# Usando um item
jogador.usar_item(item1)

# Iniciando outra batalha
batalha(jogador, monstro2)
