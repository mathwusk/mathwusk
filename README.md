python
import pygame
import os

class MusicPlayer:
    def __init__(self):
        pygame.init()
        pygame.mixer.init()
        self.playlist = []
        self.current_song = 0

    def add_song(self, song_path):
        if os.path.exists(song_path):
            self.playlist.append(song_path)
            print(f"Adicionada a música: {song_path}")
        else:
            print(f"O arquivo '{song_path}' não foi encontrado.")

    def list_songs(self):
        if self.playlist:
            print("Lista de músicas:")
            for index, song in enumerate(self.playlist):
                print(f"{index + 1}. {song}")
        else:
            print("Nenhuma música na lista.")

    def play(self, song_index=0):
        if self.playlist:
            if 0 <= song_index < len(self.playlist):
                self.current_song = song_index
                pygame.mixer.music.load(self.playlist[self.current_song])
                pygame.mixer.music.play()
                print(f"Reproduzindo: {self.playlist[self.current_song]}")
            else:
                print("Índice de música inválido.")
        else:
            print("Nenhuma música na lista para reproduzir.")

    def stop(self):
        pygame.mixer.music.stop()
        print("Reprodução interrompida.")

    def next_song(self):
        self.current_song = (self.current_song + 1) % len(self.playlist)
        self.play(self.current_song)

    def previous_song(self):
        self.current_song = (self.current_song - 1) % len(self.playlist)
        self.play(self.current_song)

    def exit_player(self):
        pygame.mixer.quit()
        pygame.quit()
        print("O aplicativo de música foi encerrado.")

if __name__ == "__main__":
    music_player = MusicPlayer()

    while True:
        print("\n===== Aplicativo de Música =====")
        print("1. Adicionar música")
        print("2. Listar músicas")
        print("3. Reproduzir música")
        print("4. Parar reprodução")
        print("5. Próxima música")
        print("6. Música anterior")
        print("7. Sair")

        choice = input("Escolha uma opção: ")

        if choice == "1":
            song_path = input("Digite o caminho da música: ")
            music_player.add_song(song_path)
        elif choice == "2":
            music_player.list_songs()
        elif choice == "3":
            music_player.play(int(input("Digite o índice da música: ")) - 1)
        elif choice == "4":
            music_player.stop()
        elif choice == "5":
            music_player.next_song()
        elif choice == "6":
            music_player.previous_song()
        elif choice == "7":
            music_player.exit_player()
            break
        else:
            print("Opção inválida. Escolha novamente.")
