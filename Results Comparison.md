# Results Summary

This document presents the experimental results for enhancing Thai sentiment analysis using WangChanBERTa with Typhoon-generated synthetic data. Two experiments were conducted to determine the optimal data augmentation strategy.

---

## Experiment 1: Determining Optimal Prompt Method Synthesis Ratio

### Objective

This experiment aims to identify the optimal synthesis ratio for the Prompt-based method by testing different augmentation levels (1x, 2x, 3x, and 4x the original dataset size).

### Results: Macro Average Performance

<table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: center;">
  <thead>
    <tr style="background-color: #d9d9d9;">
      <th rowspan="2">Synthesis Ratio</th>
      <th colspan="3">Macro Average (%)</th>
    </tr>
    <tr style="background-color: #d9d9d9;">
      <th>Precision</th>
      <th>Recall</th>
      <th>F1-score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left;">Baseline (No augmentation)</td>
      <td>88.69</td>
      <td>87.55</td>
      <td>88.11</td>
    </tr>
    <tr>
      <td style="text-align: left;">Prompt 1x</td>
      <td>90.11</td>
      <td>87.15</td>
      <td>88.56</td>
    </tr>
    <tr>
      <td style="text-align: left;">Prompt 2x</td>
      <td>89.21</td>
      <td>86.31</td>
      <td>87.69</td>
    </tr>
    <tr style="background-color: #e6f3ff;">
      <td style="text-align: left;"><strong>Prompt 3x (Best)</strong></td>
      <td><strong>92.89</strong></td>
      <td><strong>86.31</strong></td>
      <td><strong>89.28</strong></td>
    </tr>
    <tr>
      <td style="text-align: left;">Prompt 4x</td>
      <td>93.23</td>
      <td>85.29</td>
      <td>88.80</td>
    </tr>
  </tbody>
</table>

### Findings

- The **3x synthesis ratio** achieves the best overall performance with Macro F1-score of 89.28%
- The 3x ratio is selected for use in Experiment 2

---

## Experiment 2: Comparing Data Synthesis Approaches

### Objective

This experiment compares four different data augmentation strategies to identify which approach yields the best sentiment classification performance.

### Results: Macro Average Performance

<table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; text-align: center;">
  <thead>
    <tr style="background-color: #d9d9d9;">
      <th rowspan="2">Synthesis Method</th>
      <th colspan="3">Macro Average (%)</th>
    </tr>
    <tr style="background-color: #d9d9d9;">
      <th>Precision</th>
      <th>Recall</th>
      <th>F1-score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left;">No Augmentation (Baseline)</td>
      <td>88.69</td>
      <td>87.55</td>
      <td>88.11</td>
    </tr>
    <tr>
      <td style="text-align: left;">Rewritten Method (4x)</td>
      <td>88.48</td>
      <td>87.54</td>
      <td>88.00</td>
    </tr>
    <tr>
      <td style="text-align: left;">Prompt Method (3x)</td>
      <td>92.89</td>
      <td>86.31</td>
      <td>89.28</td>
    </tr>
    <tr style="background-color: #e6ffe6;">
      <td style="text-align: left;"><strong>Mixed Method (Best)</strong></td>
      <td><strong>93.63</strong></td>
      <td><strong>88.68</strong></td>
      <td><strong>90.98</strong></td>
    </tr>
  </tbody>
</table>

### Findings

- Only the **Prompt Method** and **Mixed Method** successfully improve Macro F1-score performance compared to baseline
- The **Rewritten Method** actually performs slightly worse than baseline (Macro F1-score: 88.00% vs 88.11%), suggesting that rewriting existing reviews does not provide sufficient diversity to improve model performance
- The **Mixed Method** achieves the best overall performance with Macro F1-score of 90.98%

---

## Class-wise Performance Analysis

This section examines how each synthesis method affects prediction performance for individual sentiment classes (Negative and Positive).

<table>
  <tr>
    <th>Augmentation (Synthesis) Method</th>
    <th>Class</th>
    <th>Precision (%)</th>
    <th>Recall (%)</th>
    <th>F1-Score (%)</th>
  </tr>

  <tr>
    <td rowspan="2">No Augmentation</td>
    <td>Negative</td>
    <td>78.92</td>
    <td>76.44</td>
    <td>77.66</td>
  </tr>
  <tr>
    <td>Positive</td>
    <td>98.46</td>
    <td>98.66</td>
    <td>98.56</td>
  </tr>

  <tr>
    <td rowspan="2">Rewritten Method</td>
    <td>Negative</td>
    <td>78.49 (-0.43)</td>
    <td>76.44 (0.00)</td>
    <td>77.45 (-0.21)</td>
  </tr>
  <tr>
    <td>Positive</td>
    <td>98.46 (0.00)</td>
    <td>98.63 (-0.03)</td>
    <td>98.55 (-0.01)</td>
  </tr>

  <tr>
    <td rowspan="2">Prompt Method</td>
    <td>Negative</td>
    <td>87.50 (+8.58)</td>
    <td>73.30 (-3.14)</td>
    <td>79.77 (+2.11)</td>
  </tr>
  <tr>
    <td>Positive</td>
    <td>98.27 (-0.19)</td>
    <td>99.32 (+0.66)</td>
    <td>98.79 (+0.23)</td>
  </tr>

  <tr>
    <td rowspan="2"><strong><em>Mixed Method</em></strong></td>
    <td><strong><em>Negative</em></strong></td>
    <td><strong><em>88.69 (+9.77)</em></strong></td>
    <td><strong><em>78.01 (+1.57)</em></strong></td>
    <td><strong><em>83.01 (+5.35)</em></strong></td>
  </tr>
  <tr>
    <td><strong><em>Positive</em></strong></td>
    <td><strong><em>98.57 (+0.11)</em></strong></td>
    <td><strong><em>99.35 (+0.69)</em></strong></td>
    <td><strong><em>98.96 (+0.40)</em></strong></td>
  </tr>

</table>

### Key Observations

- **Negative Class Performance**: The Mixed Method shows the most significant improvement in negative class F1-score, increasing from 77.66% (baseline) to 83.01% (+5.35 percentage points)
- **Positive Class Performance**: All three methods maintain high performance on the positive class (F1-score >98%)
- **Class Imbalance**: The Prompt and Mixed methods effectively address the class imbalance problem by substantially improving minority class (negative) prediction
- **Rewritten Method Limitation**: Actually performs worse than baseline for the negative class (F1-score: 77.45% vs 77.66%), indicating that simply rewriting existing reviews does not provide sufficient diversity and may even introduce noise

---

## Summary

### Experiment 1 Conclusion

The **3x synthesis ratio** for the Prompt-based method provides the optimal balance between data augmentation and model performance, achieving Macro F1-score of 89.28%.

### Experiment 2 Conclusion

The **Mixed Method** (combining 4x Rewritten + 3x Prompt-generated reviews) achieves the best overall performance:

- Highest Macro F1-score: 90.98%
- Significant improvement in negative class F1-score (77.66% → 83.01%)
- Maintains strong positive class F1-score (98.56% → 98.96%)

### Recommendation

For Thai sentiment analysis tasks with class imbalance, the Mixed Method approach is recommended as it effectively combines data diversity from Prompt-generated reviews with structural similarity from Rewritten reviews.
