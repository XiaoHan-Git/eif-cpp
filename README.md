## Extended Isolation Forest

C++ Implementation of [**Extended Isolation Forest**](https://doi.org/10.1109/TKDE.2019.2947676).

This is an extended version of this [standard isolated forest](https://github.com/petrkovac/iforest). Compared with the standard isolated forest, the efficiency of the algorithm is reduced, but it alleviates the problems mentioned in the paper.

For an *N* dimensional dataset, selecting a random slope for the branch cut is the same as choosing a normal vector, *n*⃗ , uniformly over the unit *N*-Sphere. This can easily be accomplished by drawing a random number for each coordinate of *n*⃗ from the standard normal distribution N(0,1).

## Installation

Just include `EIF.h` in your project and you are ready to go.

## Example

```c++
#include <iostream>
#include <stdint.h>
#include <random>
#include <vector>
#include <string>
#include "EIF.h"
#include <chrono>

int main(int argc, char* argv[])
{
	std::mt19937 rng(12345);
	std::vector<std::array<uint32_t, 4>> data;
	// generate some random 4D datapoints
	for (uint32_t i = 0; i < 1000; i++)
	{
		std::array<uint32_t, 4> temp;
		for (uint32_t j = 0; j < 4; j++)
		{
			temp[j] = EIF::UniformRandomNumber<float>::GenerateNext(rng, 0, 1000);
		}
		data.push_back(temp);
	}
    
	// add a few anomalies
	for (uint32_t i = 0; i < 5; i++)
	{
		std::array<uint32_t, 4> temp;
		for (uint32_t j = 0; j < 4; j++)
		{
			temp[j] = EIF::UniformRandomNumber<float>::GenerateNext(rng, 900, 1500);
		}
		data.push_back(temp);
	}
    
    auto t1 = std::chrono::system_clock::now();
	EIF::EIForest<uint32_t, 4> forest;
	if (!forest.Build(50, 12345, data, 100))
	{
		std::cerr << "Failed to build Isolation Forest.\n";
		return 1;
	}
	std::vector<double> anomaly_scores;
	if (!forest.GetAnomalyScores(data, anomaly_scores))
	{
		std::cerr << "Failed to calculate anomaly scores.\n";
		return 2;
	}
	for (uint32_t i = 995; i < 1005; i++)
	{
		std::cout << "Anomaly_score[" << i << "] " << anomaly_scores[i] << "\n";
	}
    auto t2 = std::chrono::system_clock::now();
    std::cout<< "time: "<<std::chrono::duration_cast<std::chrono::milliseconds>(t2 - t1).count()<<std::endl;
}
```

### Example output

```
Anomaly_score[995] 0.556233
Anomaly_score[996] 0.446035
Anomaly_score[997] 0.523133
Anomaly_score[998] 0.458597
Anomaly_score[999] 0.532598
Anomaly_score[1000] 0.588844
Anomaly_score[1001] 0.641518
Anomaly_score[1002] 0.639913
Anomaly_score[1003] 0.626346
Anomaly_score[1004] 0.639888
time: 84 milliseconds
```







