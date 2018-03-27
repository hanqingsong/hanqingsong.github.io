---
title: java List转换为树形结构数据
date: 2018-03-27 14:20:47
categories: java
tags: list
---

```
    /**
     * 转换为树形结构数据
     *
     * @param authMenuVOList 列表数据
     * @return 树形列表数据
     */
    private List<AuthMenuVO> transListToTreeList(List<AuthMenuVO> authMenuVOList) {
        List<AuthMenuVO> rootMenuList = Lists.newArrayList();
        Map<Long, AuthMenuVO> menuNodeMap = Maps.newHashMap();
        for (AuthMenuVO authMenuVO : authMenuVOList) {
            menuNodeMap.put(authMenuVO.getId(), authMenuVO);
        }
        //遍历列表数据，添加到父节点下，如果是根节点存入根节点列表中，如果不是根节点添加到父节点下
        for (AuthMenuVO authMenuVO : authMenuVOList) {
            Long pid = authMenuVO.getPid();
            //判断是否是跟节点
            if (pid == 0) {
                rootMenuList.add(authMenuVO);
            } else {
                AuthMenuVO parentMenu = menuNodeMap.get(pid);
                List<AuthMenuVO> childrenList = parentMenu.getChildren();
                if (childrenList == null) {
                    childrenList = Lists.newArrayList();
                    parentMenu.setChildren(childrenList);
                }
                childrenList.add(authMenuVO);
            }
        }

        return rootMenuList;
    }
```