# Doris桌面版发布说明

## 1.1版
- 实现Doris规则1.1版。
    - **增强的可视化功能**  
          在现有**文本输出**的基础上，新增以下三种视图:
              - **列表式报告**: 以交互式表格形式展示UCOD选择步骤。用户可逐步跟随操作流程，相应规则会在证明书上高亮显示。
              - **规则流转报告**: 直观展示导致最终选定UCOD的规则应用路径。
              - **规则顺序报告**: 通过水平时间轴按触发顺序呈现应用的规则。
    - **提升算法准确性**  
      本次升级优化了DORIS算法，显著提高了死因编码和报告的准确性与一致性：
              - 优化了感染性疾病、肿瘤和损伤相关情况的特异性。
              - 改进了HIV、结核病和物质中毒的处理逻辑。
              - 更好地处理了外因和损伤相关的死亡。
     - **新增macOS版本支持**  
      扩展系统兼容性，支持苹果操作系统用户使用。
       

## 1.0版（初始版本）

-	新增文本证明书支持功能。该工具能够高效处理海量数据，仅需几分钟即可处理数千份文本证明书。通过此更新，用户可快速获取结果，包括相应的ICD-11编码、实体名称和计算得出的根本死因（UCOD）。
-	我们投入了大量精力改进网页版用户界面。这些改进旨在提供更直观、用户友好的体验，使用户能够轻松浏览单份证明书，并将处理后的证明书保存到本地计算机。 
-	通过纳入表B中的字段（如死亡方式、妊娠等），扩展了DORIS的分析能力。通过考虑这些额外字段，DORIS现在可以进行更全面、更准确的分析，从而提高UCOD选择的质量。
-	新增了额外的控制检查和警告功能。这些新特性有助于提高结果的准确性和可靠性。
-	新增了与ANACOD-3兼容的输出选项作为DORIS的附加输出功能。此更新确保了与其他系统和工作流程更轻松的集成，提高了数据处理过程的整体效率。
-	根据MRD研讨会获得的宝贵反馈，我们对数字化死因规则库进行了重大完善和澄清。这些改进进一步优化了DORIS的性能和可靠性。
-	DORIS在线用户指南现已发布。
-   该工具所使用的规则现在通过我们的"死因规则编辑平台"进行维护。
