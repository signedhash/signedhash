> [!NOTE]
> Welcome to my GitHub page! <br>
> You can find more information on my website · [signedhash.github.io](https://signedhash.github.io)

### </about me>
I am interested in forensics, work on various projects, and study different encryption methods. The goal of this account is to make a contribution to the world.

```cpp
#include <algorithm>
#include <cmath>
#include <complex>
#include <iomanip>
#include <iostream>
#include <vector>
using namespace std;
using C = complex<double>;
constexpr double PI = 3.14159265358979323846;

void fft(vector<C>& a) {
    size_t n = a.size();
    if (n < 2) return;
    vector<C> even(n / 2), odd(n / 2);
    for (size_t i = 0; i < n / 2; ++i) even[i] = a[2 * i], odd[i] = a[2 * i + 1];
    fft(even); fft(odd);
    for (size_t k = 0; k < n / 2; ++k) {
        C t = polar(1.0, -2 * PI * k / n) * odd[k];
        a[k] = even[k] + t;
        a[k + n / 2] = even[k] - t;
    }
}

int main() {
    constexpr size_t N = 1024;
    constexpr double rate = 1024.0;
    vector<C> signal(N);
    for (size_t i = 0; i < N; ++i) {
        double t = i / rate;
        signal[i] = sin(2 * PI * 64 * t) + 0.5 * sin(2 * PI * 180 * t);
    }
    fft(signal);
    vector<pair<double, size_t>> peaks;
    for (size_t k = 1; k < N / 2; ++k) {
        double amplitude = 2.0 * abs(signal[k]) / N;
        if (amplitude > 0.01) peaks.emplace_back(amplitude, k);
    }
    sort(peaks.rbegin(), peaks.rend());
    cout << "FFT / signal spectrum\nSamples: " << N << " | sample rate: " << rate << " Hz\n\n";
    cout << fixed << setprecision(2) << "Detected frequency components:\n";
    for (auto [amplitude, bin] : peaks)
        cout << "  " << bin * rate / N << " Hz  | amplitude " << setprecision(3) << amplitude << setprecision(2) << '\n';
    cout << "\nFFT complexity: O(N log N)\n";
}
```
