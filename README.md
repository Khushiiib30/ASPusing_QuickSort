#include <stdio.h>

// Structure to represent an activity
struct Activity {
    int start;
    int finish;
};

// Function to partition the array for Quicksort
int partition(struct Activity activities[], int low, int high) {
    int pivot = activities[high].finish;
    int i = low - 1;

    for (int j = low; j <= high - 1; j++) {
        if (activities[j].finish < pivot) {
            i++;
            // Swap activities[i] and activities[j]
            struct Activity temp = activities[i];
            activities[i] = activities[j];
            activities[j] = temp;
        }
    }

    // Swap activities[i+1] and activities[high] (pivot)
    struct Activity temp = activities[i + 1];
    activities[i + 1] = activities[high];
    activities[high] = temp;

    return i + 1;
}

// Function to perform Quicksort on the activities array
void quickSort(struct Activity activities[], int low, int high) {
    if (low < high) {
        int pi = partition(activities, low, high);

        quickSort(activities, low, pi - 1);
        quickSort(activities, pi + 1, high);
    }
}

// Function to print selected activities
void printSelectedActivities(struct Activity activities[], int n) {
    int i, j;

    // The first activity is always selected
    i = 0;
    printf("Selected Activities: (%d, %d)\n", activities[i].start, activities[i].finish);

    // Consider the rest of the activities
    for (j = 1; j < n; j++) {
        // If this activity has a start time greater than or equal to the finish time of the previous activity, then select it
        if (activities[j].start >= activities[i].finish) {
            printf("(%d, %d)\n", activities[j].start, activities[j].finish);
            i = j;
        }
    }
}

int main() {
    int n;

    printf("Enter the number of activities: ");
    scanf("%d", &n);

    struct Activity activities[n];

    printf("Enter start and finish times for each activity:\n");
    for (int i = 0; i < n; i++) {
        printf("Activity %d: ", i + 1);
        scanf("%d %d", &activities[i].start, &activities[i].finish);
    }

    // Perform Quicksort to sort the activities by finish times
    quickSort(activities, 0, n - 1);

    printf("Activities sorted by finish times:\n");
    for (int i = 0; i < n; i++) {
        printf("(%d, %d) ", activities[i].start, activities[i].finish);
    }
    printf("\n");

    printf("\nMaximum number of non-overlapping activities:\n");
    printSelectedActivities(activities, n);

    return 0;
}
