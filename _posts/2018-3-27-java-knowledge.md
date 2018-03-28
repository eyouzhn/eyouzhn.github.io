---
layout:     post
title:      Java 知识要点
subtitle:     Java 知识要点
date:       2018-03-27
author:     BY Jeff Zhan
header-img: img/post-bg-BJJ.jpg
catalog: true
tags:
    - 技术栈
---
1. RowBounds用法    
    
   // 构造分页对象
    RowBounds rowBounds =
        new RowBounds(PageUtils.getOffset(itemSearch.getCurrentPage(), itemSearch.getPageSize()),
            PageUtils.getLimit(itemSearch.getPageSize()));
    // 查询节点管理分页列表
    List<MgmtNodeExt> result = mgmtNodeDao.getMgmtNodeList(criteria, rowBounds);
    
2、 API 定义：
  @ApiOperation("保存模型版本结果数据类型")
  @RequestMapping(value = "/saveAlgVersResult", method = RequestMethod.POST)
  public ResponseDto addAlgVersResult(@RequestBody AlgVersResultAddVo algVersResultVo) {
    String methodName = "saveAlgVersResult";
    
3. SaltStack:
自动化运维工具SaltStack详细部署
http://blog.51cto.com/sofar/1596960

4.
  @Override
  @Transactional(readOnly=true,propagation = Propagation.REQUIRES_NEW)
  public boolean checkPreviousProcess(Integer comModelVersionPk, Integer comTargetMgmntNodePk,
      String communityId, UserContext usrCtx) throws ModelMgmtException {
      
5.
  /**********************************************************
   * This method only used by JOB, so the transaction propagation type is REQUIRES_NEW
   */
  @Override
  @Transactional(propagation = Propagation.REQUIRES_NEW)
  
  https://www.cnblogs.com/xuxiuxiu/p/7080660.html  
  
  
6. left join & 
	<select id="getTaskAndModelAndModelVersionByTaskPk" resultMap="BaseResultMap"
		parameterType="java.lang.Integer">
		select
		task.*,amv.alg_model_pk,amv.version_no,amv.description,amv.version_status,amv.file_upload_flag,
		am.name,am.code,am.node_type,am.runtime_type,am.model_cat,am.model_status,am.latest_version_no,
		am.event_type_list,am.realtime_flag,atp.name as task_plan_name
		from "mmc".alg_task task
		left join "mmc".alg_model_version amv on task.alg_model_version_pk =
		amv.alg_model_version_pk
		left join "mmc".alg_model am on
		am.alg_model_pk = amv.alg_model_pk
		left join "mmc".alg_task_plan atp on
		atp.alg_task_plan_pk = task.alg_task_plan_pk
		where
		task.delete_flag = ${@com.eg.egsc.egc.modelmgmt.dao.common.ModelMgmtDaoConstant@INT_FALSE_DELFLAG}
		and task.alg_task_pk = #{algTaskPk,jdbcType=INTEGER}
	</select>    

7. applicationContext.getBean(beanKey, clz);
https://blog.csdn.net/zghwaicsdn/article/details/50910384
	
8. Spring Test
@RunWith(SpringRunner.class)
@SpringBootTest(classes = {ModelmgmtServiceApplication.class})	

9. ETL 数据抽取

10. Java UT

	@Test
	@Transactional
	@Rollback(true)
	public void testCount() {
		Role role = createRole();
		RoleBaseDto roleBaseDto = new RoleBaseDto();
		roleBaseDto.setUuid(role.getUuid());
		roleBaseDto.setRoleName(role.getRoleName());
		int count = roleServiceImpl.count(roleBaseDto);
		assertEquals(true, count > 0);
	}
	
11. 	Point Cut 假如想在某个文件夹下面的所有函数的入口和出口增加 日志打印，应该怎么做？
https://blog.csdn.net/rainbow702/article/details/52185827