#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define MAX_SONGS 10
#define TIME_QUANTUM 3

typedef struct {
    char title[50];
    char artist[50];
    int duration; // in seconds
} Song;

Song songs[MAX_SONGS] = {
    {"Song A", "Artist 1", 5}, {"Song B", "Artist 2", 3},
    {"Song C", "Artist 3", 4}, {"Song D", "Artist 4", 2},
    {"Song E", "Artist 5", 6}, {"Song F", "Artist 6", 7},
    {"Song G", "Artist 7", 5}, {"Song H", "Artist 8", 3},
    {"Song I", "Artist 9", 4}, {"Song J", "Artist 10", 2}
};

// Function to display available songs
void displaySongs() {
    printf("\nAvailable Songs:\n");
    for (int i = 0; i < MAX_SONGS; i++) {
        printf("%d. %s by %s (%d sec)\n", i, songs[i].title, songs[i].artist, songs[i].duration);
    }
}

// Function to create a custom playlist
void createPlaylist(int playlist[], int *size) {
    displaySongs();
    printf("Enter the number of songs in your playlist (max %d): ", MAX_SONGS);
    scanf("%d", size);
    if (*size < 1 || *size > MAX_SONGS) {
        printf("Invalid size. Try again.\n");
        return;
    }
    printf("Select songs by entering their numbers (0-%d):\n", MAX_SONGS - 1);
    for (int i = 0; i < *size; i++) {
        scanf("%d", &playlist[i]);
        if (playlist[i] < 0 || playlist[i] >= MAX_SONGS) {
            printf("Invalid song number. Try again.\n");
            i--;
        }
    }
}

// Function to display the user's playlist
void displayPlaylist(int playlist[], int size) {
    if (size == 0) {
        printf("Your playlist is empty! Create one first.\n");
        return;
    }
    printf("\nYour Playlist:\n");
    for (int i = 0; i < size; i++) {
        printf("%d. %s by %s (%d sec)\n", i + 1, songs[playlist[i]].title, songs[playlist[i]].artist, songs[playlist[i]].duration);
    }
}

// Function to suggest a song randomly
void suggestSong() {
    srand(time(0));
    int index = rand() % MAX_SONGS;
    printf("Suggested song: %s by %s (%d sec)\n", songs[index].title, songs[index].artist, songs[index].duration);
}

// Function to shuffle the playlist
void shufflePlaylist(int playlist[], int size) {
    srand(time(0));
    for (int i = size - 1; i > 0; i--) {
        int j = rand() % (i + 1);
        int temp = playlist[i];
        playlist[i] = playlist[j];
        playlist[j] = temp;
    }
    printf("Playlist shuffled!\n");
}

// Function to autoplay using Round Robin scheduling
void autoplayRoundRobin(int playlist[], int size) {
    int remaining[MAX_SONGS], time = 0;
    for (int i = 0; i < size; i++) remaining[i] = songs[playlist[i]].duration;

    printf("\nAutoplaying (Round Robin, Time Quantum = %d sec):\n", TIME_QUANTUM);
    while (1) {
        int done = 1;
        for (int i = 0; i < size; i++) {
            if (remaining[i] > 0) {
                done = 0;
                if (remaining[i] > TIME_QUANTUM) {
                    time += TIME_QUANTUM;
                    remaining[i] -= TIME_QUANTUM;
                    printf("Playing %s for %d sec, Remaining: %d sec.\n",
                           songs[playlist[i]].title, TIME_QUANTUM, remaining[i]);
                } else {
                    time += remaining[i];
                    printf("Playing %s for %d sec, Completed.\n",
                           songs[playlist[i]].title, remaining[i]);
                    remaining[i] = 0;
                }
            }
        }
        if (done) break;
    }
    printf("Total time: %d sec\n", time);
}

// Function to autoplay using Shortest Job Time (SJT)
void autoplaySJT(int playlist[], int size) {
    int temp, i, j;
    for (i = 0; i < size - 1; i++) {
        for (j = i + 1; j < size; j++) {
            if (songs[playlist[i]].duration > songs[playlist[j]].duration) {
                temp = playlist[i];
                playlist[i] = playlist[j];
                playlist[j] = temp;
            }
        }
    }

    printf("\nAutoplaying (Shortest Job Time First - SJT):\n");
    int time = 0;
    for (i = 0; i < size; i++) {
        printf("Playing %s for %d sec. Completed.\n",
               songs[playlist[i]].title, songs[playlist[i]].duration);
        time += songs[playlist[i]].duration;
    }
    printf("Total time: %d sec\n", time);
}

// Function to skip to the next song
void playNextSong(int *currentSong, int size) {
    if (*currentSong < size - 1) {
        (*currentSong)++;
        printf("Skipping to next song: %s by %s\n",
               songs[*currentSong].title, songs[*currentSong].artist);
    } else {
        printf("End of playlist reached.\n");
    }
}

// Main function
int main() {
    int playlist[MAX_SONGS], size = 0, choice, currentSong = 0;

    while (1) {
        printf("\nMusic Scheduler Menu:\n");
        printf("1. Create Playlist\n");
        printf("2. View Playlist\n");
        printf("3. Suggest a Song\n");
        printf("4. Shuffle Playlist\n");
        printf("5. Autoplay (Round Robin)\n");
        printf("6. Autoplay (Shortest Job Time - SJT)\n");
        printf("7. Skip to Next Song\n");
        printf("8. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: createPlaylist(playlist, &size); break;
            case 2: displayPlaylist(playlist, size); break;
            case 3: suggestSong(); break;
            case 4: shufflePlaylist(playlist, size); break;
            case 5: if (size > 0) autoplayRoundRobin(playlist, size);
                    else printf("Create a playlist first!\n"); break;
            case 6: if (size > 0) autoplaySJT(playlist, size);
                    else printf("Create a playlist first!\n"); break;
            case 7: playNextSong(&currentSong, size); break;
            case 8: printf("Exiting...\n"); return 0;
            default: printf("Invalid choice. Try again.\n");
        }
    }
}
