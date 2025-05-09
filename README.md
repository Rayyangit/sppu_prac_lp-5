%%writefile omp_statss.cpp
#include <iostream>
#include <omp.h>
#include <climits>  // for INT_MAX and INT_MIN

using namespace std;

void min(int *arr, int n) {
    int min_value = INT_MAX;
#pragma omp parallel for reduction(min : min_value)
    for (int i = 0; i < n; i++) {
        if (arr[i] < min_value) {
            min_value = arr[i];
        }
    }
    cout << "\nMin value: " << min_value << endl;
}

void max(int *arr, int n) {
    int max_value = INT_MIN;
#pragma omp parallel for reduction(max : max_value)
    for (int i = 0; i < n; i++) {
        if (arr[i] > max_value) {
            max_value = arr[i];
        }
    }
    cout << "Max value: " << max_value << endl;
}

void avg(int *arr, int n) {
    int sum = 0;
#pragma omp parallel for reduction(+:sum)
    for (int i = 0; i < n; i++) {
        sum += arr[i];
    }
    float average = (float)sum / n;
    cout << "Sum: " << sum << endl;
    cout << "Average: " << average << endl;
}

int main() {
    omp_set_num_threads(4);
    int n;

    cout << "Enter the number of elements in the array: ";
    cin >> n;

    int *arr = new int[n];

    cout << "Enter the elements of the array: ";
    for (int i = 0; i < n; ++i) {
        cin >> arr[i];
    }

    cout << "\nArray elements are: ";
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;

    min(arr, n);
    max(arr, n);
    avg(arr, n);

    delete[] arr;
    return 0;
}

g++ -fopenmp omp_statss.cpp -o omp_statss
./omp_statss
