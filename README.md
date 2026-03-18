#include <iostream>
#include <vector>
#include <cstdlib>
#include <omp.h>

using namespace std;

void shellSort(vector<int>& arr) {
    int n = arr.size();

    for (int gap = n / 2; gap > 0; gap /= 2) {

        // Параллелим независимые группы
        #pragma omp parallel for
        for (int start = 0; start < gap; start++) {

            for (int i = start + gap; i < n; i += gap) {
                int temp = arr[i];
                int j = i;

                while (j >= gap && arr[j - gap] > temp) {
                    arr[j] = arr[j - gap];
                    j -= gap;
                }
                arr[j] = temp;
            }
        }
    }
}

int main() {
    int n = 10000; // >= 1000
    vector<int> arr(n);

    // Заполнение массива случайными числами
    for (int i = 0; i < n; i++) {
        arr[i] = rand() % 10000;
    }

    // Установка количества потоков (можешь менять)
    omp_set_num_threads(4);

    double start = omp_get_wtime();

    shellSort(arr);

    double end = omp_get_wtime();

    cout << "Time: " << end - start << " seconds" << endl;

    // Проверка (первые 10 элементов)
    for (int i = 0; i < 10; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;

    return 0;
}
