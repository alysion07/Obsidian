test text
# 마크다운
## 테스트

`v백틱 테스트 `


``` cpp
// Update Regional Segment
std::vector<DBKey*> set_rs_keys;
for (int j = 0; j < geo_count; j++)
{
	AssignedGeometryData geo_data = geo_list[j];
	ERegionalSegmentItem* p_rs_item = (ERegionalSegmentItem*)m_docs.GetDB(DB_REGIONAL_SEGMENT)->GetDBItem(geo_data.p_rs_key);

	if (p_rs_item)
	{
		// If this ERegionalSegmentItem is already set, DO NOT get the same JSON again
		auto it = std::find(set_rs_keys.begin(), set_rs_keys.end(), p_rs_item->m_KEY);
		if (it != set_rs_keys.end())  continue;

		json::JsonArray topology_arr;
		topology_arr = GetLBMView()->get_topology_as_group_json((E_VIEW::object_pointer)p_rs_item->GetObjData());
		p_rs_item->SetTopologyJsonArray(topology_arr);

		set_rs_keys.push_back(p_rs_item->m_KEY);
	}
}
`````