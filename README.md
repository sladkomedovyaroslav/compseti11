#include <iostream>
#include <vector>
#include <cstdlib>
#include <omp.h>
#include <ctime>
using namespace std;

void shellSort(vector<int>& arr) { // фунция сортировки шелла
    int n = arr.size(); // колво элем в массиве

    for (int gap = n / 2; gap > 0; gap /= 2) { //основной цикл

        
        #pragma omp parallel for
        for (int start = 0; start < gap; start++) { //формируем независимых последователностей элементов которые можно сортировать одновременно 

            for (int i = start + gap; i < n; i += gap) { //идет по элементам текущей последовательности увеличиваем индекс 
                int temp = arr[i]; // сохраняем текущий элемент 
                int j = i;

                while (j >= gap && arr[j - gap] > temp) { //сортировка по шагу гап 
                    arr[j] = arr[j - gap]; //если предыдущий элем больше темп сдвигаем его на гап вправо 
                    j -= gap; //идем к след элементу 
                }
                arr[j] = temp; //ставим элемент на правильное место
            }
        }
    }
}

int main() {
    srand(time(0));
    int n = 10000; 
    vector<int> arr(n);
    
    for (int i = 0; i < n; i++) {
        arr[i] = rand() % 10000;
    }

   
    omp_set_num_threads(4); //задаем колво потоков 

    double start = omp_get_wtime(); //засекаем время 

    shellSort(arr);

    double end = omp_get_wtime();

    cout << "Time: " << end - start << " seconds" << endl;

    
    for (int i = 0; i < 10; i++) { //проверка сортировки
        cout << arr[i] << " ";
    }
    cout << endl;

    return 0;
}
