- 防災應變

- 能源永續

- 健康守護
    - 登革熱好發區域
    - football / basketball / baseball fields / parks 分佈

- 商圈活化

- 全民學習

- 多元融合


select * from (
select 
    行政區 as x_axis,
    case
	when 抵達時間 between 0 and 1659 then '1700前'
	when 抵達時間 between 1700 and 1859 then '1700-1900'
	when 抵達時間 between 1900 and 2059 then '1900-2100'
	else '2100後'
    end as y_axis,
    count(*) as data
from garbage_truck
group by
    行政區,
    case 
	when 抵達時間 between 0 and 1659 then '1700前'
	when 抵達時間 between 1700 and 1859 then '1700-1900'
	when 抵達時間 between 1900 and 2059 then '1900-2100'
	else '2100後'
    end
) as t
order by
    array_position(array['北投區', '士林區', '內湖區', '南港區', '松山區', '信義區', '中山區', '大同區', '中正區', '萬華區', '大安區', '文山區']::varchar[], t.x_axis),
    array_position(array['1700前', '1700-1900', '1900-2100', '2100後'], t.y_axis);



insert into public.query_charts (index, history_config, map_config_ids, map_filter, time_from, time_to, update_freq, update_freq_unit, source, short_desc, long_desc, use_case, links, contributors, created_at, updated_at, query_type, query_chart, query_history, city)
values ('garbage_truck', NULL, NULL, NULL, 'static', NULL, NULL, NULL, '環保局', NULL, NULL, NULL, '{}', '{tuic}', '2025-05-30 17:00:00+00','2025-05-30 17:00:00+00', 'three_d', 'select * from ( select 行政區 as x_axis, case when 抵達時間 between 0 and 1659 then ''1700前'' when 抵達時間 between 1700 and 1859 then ''1700-1900'' when 抵達時間 between 1900 and 2059 then ''1900-2100'' else ''2100後'' end as y_axis, count(*) as data from garbage_truck group by 行政區, case when 抵達時間 between 0 and 1659 then ''1700前'' when 抵達時間 between 1700 and 1859 then ''1700-1900'' when 抵達時間 between 1900 and 2059 then ''1900-2100'' else ''2100後'' end) as t order by array_position(array[''北投區'', ''士林區'', ''內湖區'', ''南港區'', ''松山區'', ''信義區'', ''中山區'', ''大同區'', ''中正區'', ''萬華區'', ''大安區'', ''文山區'']::varchar[], t.x_axis), array_position(array[''1700前'', ''1700-1900'', ''1900-2100'', ''2100後''], t.y_axis);', NULL, 'taipei');

delete from public.query_charts where index='garbage_truck';

