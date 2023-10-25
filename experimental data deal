import re
import math
import datetime
import numpy as np
import pandas as pd
import openpyxl
import os
from itertools import groupby

start_time = datetime.datetime.now()
print('程序开始时间：%s' % (start_time))
##应变处理
index_path_file = 'E:/OneDrive - xxx/插图/strain/nodeindex-zlqb_2.xlsx'
index_zlqb_2  = pd.read_excel(index_path_file, sheet_name='node-index' ,index_col=[0], header=[0])
index_zlqb_2 = index_zlqb_2['index']

data_path_file = 'E:/OneDrive - xxx/插图/strain/zlqb_2.xlsx'
data_zlqb_2  = pd.read_excel(data_path_file, header=[0])
Displacement_zlqb_2 = data_zlqb_2['U']

three_cycle = 8
two_cycle = 1
one_cycle = 1

#name_gauges_list = ['3_6', '3_7', '14_2', '3_10', '12_1', '14_4', '12_4', '12_5', '14_6', '15_7', '15_8', '5_7', '15_9', '15_10', '5_8', '28_7', '28_8', '34_5', '28_9', '28_10', '34_6']
#name_gauges_list = ['14_9', '14_10', '5_1', '15_1', '15_2', '5_2', '5_9', '5_10', '34_1', '28_1', '28_2', '34_2']
name_gauges_list = ['6_3', '6_5', '6_6', '6_8']
peak_strain = pd.DataFrame()
for i in range(0, len(name_gauges_list)):
    name_gauge = name_gauges_list[i]
    data_guage = data_zlqb_2['%s' %name_gauge]
    data_guage = data_guage.sub(data_guage.iloc[0])
    gague_data = pd.DataFrame()
    for j in range(0, 10):
        if j < three_cycle:
            start_index = index_zlqb_2[12*j]
            end_index = index_zlqb_2[12*(j+1)]
        elif j == 8:
            start_index = index_zlqb_2[96]
            end_index = index_zlqb_2[104]
        else:
            start_index = index_zlqb_2[104]
            end_index = index_zlqb_2[108]
        data_level = data_guage[start_index : end_index+1]
        max_strain = data_level.nlargest(10).iloc[-1]
        max_index = data_level[data_level == max_strain].index[0]
        min_strain = data_level.nsmallest(10).iloc[-1]
        min_index = data_level[data_level == min_strain].index[0]
        new_maxdis = Displacement_zlqb_2[max_index]
        new_mindis = Displacement_zlqb_2[min_index]
        new_strain = pd.DataFrame(({'U': [new_maxdis, new_mindis], 'Strain': [max_strain, min_strain]}))
        new_strain.columns = pd.MultiIndex.from_product([['%s' %name_gauge], new_strain.columns])
        gague_data = pd.concat([gague_data, new_strain], axis=0)
    #gague_data = gague_data.sort_values(by=[('%s' %name_gauge, 'U'),  ('%s' %name_gauge, 'Strain')])
    gague_data = gague_data.reset_index(drop=True)
    peak_strain = pd.concat([peak_strain, gague_data], axis=1)
with pd.ExcelWriter('E:/OneDrive - xxx//插图/strain/gaguesstrain-zlqb_2.xlsx', mode='a') as writer:
    peak_strain.to_excel(writer, sheet_name='wall_guages')
