# C-PROGRAMMING-PROJECT
Project made using c programming concepts. 

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

struct Song {
    char title[50];
    char artist[50];
    char genre[30];
};

struct Song playlist[100];
int count = 0;

void addSong() {
    printf("Enter Title: ");
    scanf(" %[^\n]", playlist[count].title);

  printf("Enter Artist: ");
    scanf(" %[^\n]", playlist[count].artist);

   printf("Enter Genre: ");
    scanf(" %[^\n]", playlist[count].genre);

  count++;
    printf("Song Added Successfully!\n");
}

void viewSongs() {
    if (count == 0) {
        printf("\nNo songs in playlist.\n");
        return;
    }

  printf("\n------ PLAYLIST ------\n");
    for (int i = 0; i < count; i++) {
        printf("%d. %s | %s | %s\n", i + 1, playlist[i].title, playlist[i].artist, playlist[i].genre);
    }
}

void deleteSong() {
    viewSongs();
    if (count == 0) return;

  int index;
    printf("\nEnter song number to delete: ");
    scanf("%d", &index);

  if (index < 1 || index > count) {
        printf("Invalid choice.\n");
        return;
    }

  for (int i = index - 1; i < count - 1; i++) {
        playlist[i] = playlist[i + 1];
    }

   count--;
    printf("Song Deleted Successfully!\n");
}

void searchSong() {
    char key[50];
    printf("Enter song name to search: ");
    scanf(" %[^\n]", key);

  for (int i = 0; i < count; i++) {
        if (stricmp(playlist[i].title, key) == 0) {
            printf("\nFound: %s | %s | %s\n", playlist[i].title, playlist[i].artist, playlist[i].genre);
            return;
        }
    }
    printf("Song not found.\n");
}

void shuffleSongs() {
    if (count < 2) {
        printf("Not enough songs to shuffle.\n");
        return;
    }

   srand(time(0));
    for (int i = 0; i < count; i++) {
        int r = rand() % count;
        struct Song temp = playlist[i];
        playlist[i] = playlist[r];
        playlist[r] = temp;
    }
    printf("Playlist Shuffled Successfully!\n");
}

void savePlaylist() {
    FILE *fp = fopen("playlist.txt", "w");

   for (int i = 0; i < count; i++) {
        fprintf(fp, "%s|%s|%s\n", playlist[i].title, playlist[i].artist, playlist[i].genre);
    }

   fclose(fp);
    printf("Playlist Saved!\n");
}

void loadPlaylist() {
    FILE *fp = fopen("playlist.txt", "r");
    if (!fp) return;

   count = 0;
    while (fscanf(fp, " %[^|]|%[^|]|%[^\n]\n",
                  playlist[count].title,
                  playlist[count].artist,
                  playlist[count].genre) != EOF) {
        count++;
    }

    fclose(fp);
}

int main() {
    loadPlaylist();
    int choice;

  while (1) {
        printf("\n--- MUSIC PLAYLIST MANAGER ---\n");
        printf("1. Add Song\n");
        printf("2. View Songs\n");
        printf("3. Delete Song\n");
        printf("4. Search Song\n");
        printf("5. Shuffle Playlist\n");
        printf("6. Save Playlist\n");
        printf("7. Exit\n");
        printf("Choose: ");
        scanf("%d", &choice);

   switch (choice) {
            case 1: addSong(); break;
            case 2: viewSongs(); break;
            case 3: deleteSong(); break;
            case 4: searchSong(); break;
            case 5: shuffleSongs(); break;
            case 6: savePlaylist(); break;
            case 7: printf("Goodbye!\n"); return 0;
            default: printf("Invalid choice.\n");
        }
    }
}
