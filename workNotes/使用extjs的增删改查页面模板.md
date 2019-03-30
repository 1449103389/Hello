```Javascript
/**
 * @Title: xxx.js
 * @Package:
 * @Description:
 * @Author: xxx
 * @Date: xxx
 * @version V1.0
 */
//配置引入
Ext.Loader.setConfig({
    enabled : true,
    mixins: ['Devp.business.BusinessBase'],
    paths : {
        'Devp.mdm.components': basepath + '/views/mdm/components',
        'Devp.bizcommon.components': basepath + '/views/bizcommon/components'
    }
});
//js类定义
Ext.define('Devp.dh.basedata.OperationBaseInfo', {
    extend: 'Devp.business.Base',
    mixins: ['Devp.business.BusinessBase'],
    requires : ['Devp.mdm.components.MdmComboBox'],
    config: {
        renderTo: Ext.getBody()
    },
    
    //类变量区
    table: null,
    fields: null,
    columns: null,
    message: null,
    addBtn: null,//新增按钮
    editBtn: null,//编辑按钮
    delBtn: null,//删除按钮
    viewBtn: null,//查看按钮
    exportDataBtn: null,//导出按钮
    listUrl: basepath + '/xxx/listmazypage.ajson?',//表格url
    addUrl: basepath + '/xxx/insert.ajson?',//添加url
    editUrl: basepath + '/xxx/update.ajson?',//编辑url
    delUrl: basepath + '/xxx/delete.ajson?',//删除url
    viewUrl: basepath + '/xxx/queryInfo.ajson?',//查看url
    
    //初始化函数
    initItem: function () {
        var me = this;
        //创建消息框
        me.message = new Devp.comp.MessageBox();
        //创建按钮
        me.createBtns();
        //数据字段映射，mapping属性对应pageQueryResult下的对象关系映射
        me.fields = [
            {mapping: 'xxx', name: 'xxx'},
            {mapping: 'xxx', name: 'xxx'}
        ];
        //表头配置，dataIndex对应fields里的name属性
        me.columns = [
            {text: '<i18n key=xxx>油田</i18n>', dataIndex: 'projectName', width: 150},
            {text: '<i18n key=xxx>井名</i18n>', dataIndex: 'wellName', width: 150}
        ];
        //渲染函数
        function render(data){
                if(Ext.isEmpty(data)){
                    return null;
                }else{
                    return xxx;
                } 
        }
        //表格
        me.table = Ext.create('Devp.comp.PagingGrid', {
            store: me.createStore('model.page.pageQueryResult'),
            columns: {
                defaults: {align: 'left', style: 'text-align:center'},
                items: [
                    {
                        defaults: {
                            align: 'left',
                            style: 'text-align:center'
                        },
                        columns: me.columns
                    }]
            },
            enableEditPlugin: false,
            enableExportPlugin:true,
            listeners: {
                select:function(){
                    //有记录被选中使按钮可用
                    me.viewBtn.enable();
                    me.editBtn.enable();
                    me.delBtn.enable();
                },
                selectionchange:function(){
                    var selModel = me.table.getSelectionModel();
                    var selected = selModel.getSelection();
                    if (Ext.isEmpty(selected)) {
                        me.viewBtn.setDisabled(true);
                        me.editBtn.setDisabled(true);
                        me.delBtn.setDisabled(true);
                    }
                }
            }
        });
        //创建一个常规表格
        var frame = new Devp.comp.ToolBarFrame({
            // 查询控件
            searchControls: [],
            advancedSearch: true,
            advancedPanel:{
                xtype:'form',
                layout: {type: 'hbox', pack: 'center'},
                margin: '10 0 0 0',
                defaults: {labelAlign:'center'},
                //窗口控件设置
                items:[
                {
                    xtype: 'label',
                    padding: 3,
                    text: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_ENTITY_NAME>实体名</i18n>:'
                },
                {//主数据区块树
                    xtype: 'mdmcombo',
                    name: 'entityId',
                    jsClass: 'Devp.dh.basedata.OperationBaseInfo',
                    width: 170,
                    mdmId: 'C7F0847475594517BD0C32F73CE0D403'//井主数据ID TODO 后期改为字典获取
                },
                {
                    xtype: 'label',
                    padding: 3,
                    text: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_RIG>修井机</i18n>:'
                },
                {
                    name: 'rigId',
                    editable: false,
                    xtype: 'combo',
                    width: 150,
                    store:{
                        xtype:'store',
                        fields: [{name:'text',mapping:'entity.rigName'},{name:'value',mapping:'entity.identifier'}],
                        data : me.rigList
                    },
                    queryMode: 'local',
                    displayField: 'text',
                    valueField: 'value'
                },
                {
                    xtype: 'label',
                    padding: 3,
                    text: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_QUERY_START_DATE>开始时间</i18n>：'
                },
                {
                    layout:'hbox',
                    xtype:'panel',
                    items:[
                        {
                            xtype: 'datefield',
                            name: 'startDate',
                            editable: false,
                            format: 'Y-m-d',
                            width: 95
                        },
                        {xtype:'label',text:'-',padding:3},
                        {
                            xtype: 'datefield',
                            name: 'endDate',
                            editable: false,
                            format: 'Y-m-d',
                            width: 95
                        }]
                }
                ],
                bbar:new Ext.Toolbar({
                    layout: {
                        type: 'hbox',
                        pack: 'center'//查询栏布局居中
                    },
                    items: [{
                        text:'<i18n key=PORTAL_MAIN_COMMON_QUERY>查询</i18n>',
                        iconCls:'common_query',
                        handler:function(){
                            me.queryParams = {};
                            var form = this.up('form');
                            var masterData = form.form.findField('entityId').getData();
                            var masterValue = form.form.findField('entityId').getValue();
                            if(!Ext.isEmpty(masterData)){
                                switch(masterData.masterDataCode){
                                    case "ORGANIZATION": me.queryParams['orgId'] = masterValue; break;
                                    case "BLOCK": me.queryParams['projectId'] = masterValue; break;
                                    case "WELL": me.queryParams['wellId'] = masterValue; break;
                                }
                            }
                            var rigId = form.query('[name=rigId]')[0].value;
                            var startDate = form.query('[name=startDate]')[0].value;
                            var endDate = form.query('[name=endDate]')[0].value;
                            startDate = Ext.util.Format.date(startDate, 'Y-m-d');
                            endDate = Ext.util.Format.date(endDate, 'Y-m-d');
                            me.queryParams['entity.rigId'] = rigId;
                            me.queryParams['entity.startDate'] = startDate;
                            me.queryParams['entity.endDate'] = endDate;
                            me.onFindData();
                        }
                    },{
                        text:'<i18n key=ICON_ORGAUTH_RESET_NAME>重置</i18n>',
                        iconCls:'recovery',
                        handler:function(){
                            me.queryParams = {};
                            var form =this.up('form').form;
                            form.findField('rigId').setValue(null);
                            form.findField('startDate').setValue(null);
                            form.findField('endDate').setValue(null);
                            form.findField('entityId').setValue(null);
                            me.onFindData();
                        }
                    }]
                })
            },

            operatorControls: [me.addBtn, me.editBtn, me.viewBtn, me.delBtn,me.exportDataBtn],
            advancedContainerHeight : 80,
            item: me.table
        });
        me.onFindData();
        return frame.init();
    },
    
    /**
     * 表格store
     * @param root 数据源
     * @returns {Ext.data.Store}
     */
    createStore: function(root) {
        var me = this;
        //数据字段映射,mapping属性对应pageQueryResult下的对象关系映射
        var store = Ext.create('Ext.data.Store', {
            autoLoad: false,
            fields: me.fields,
            proxy: {
                type: "ajax",
                url: me.listUrl,//分页查询url
                actionMethods: {
                    create: 'POST',
                    read: 'POST',
                    update: 'POST',
                    destroy: 'POST'
                },
                reader: {
                    rootProperty: function (data) {
                        // 根据读取的属性,设置页数
                        store.pageSize = data.model.page.fetchSize;
                        return new Function('data', 'return data.' + root)(data);
                    },
                    totalProperty: 'model.page.totalCount'
                },
                pageParam: 'vo.page.pageNumber'
            }
        });
        return store;
    },
    
    /**
     * 调用store的loadPage
     */
    onFindData: function () {
        var me = this;
        me.table.store.proxy.extraParams = me.queryParams;
        me.table.store.loadPage(1);
    },
    
    /**
     * 创建新增、编辑、查看、删除按钮
     */
    createBtns: function() {
        var me = this;
        me.addBtn = Ext.create({
            xtype: 'button',
            iconCls: 'common_add',
            disabled: false,
            text: '<i18n key=METADATA_COMMON_NEW>新增</i18n>',
            handler: function () {
                var win = me.createAddWindow();
                win.show();
            }
        });
        me.editBtn = Ext.create({
            xtype: 'button',
            iconCls: 'common_edit',
            disabled: true,
            text: '<i18n key=METADATA_COMMON_EDIT>编辑</i18n>',
            handler: function () {
                var selModel = me.table.getSelectionModel();
                var selected = selModel.getSelection();
                if (Ext.isEmpty(selected)) {
                    me.message.showPopo('', '<i18n key=METADATA_COMMON_REQUIRED_WARN>请选择数据</i18n>', 2000);
                    return;
                }
                var win = me.createEditWindow(selected[0].data.entity.identifier);
                win.show();
            }
        });
        me.delBtn = Ext.create({
            xtype: 'button',
            iconCls: 'common_delete',
            disabled: true,
            text: '<i18n key=METADATA_COMMON_DELETE>删除</i18n>',
            handler: function () {
                var selModel = me.table.getSelectionModel();
                var selected = selModel.getSelection();
                if (Ext.isEmpty(selected)) {
                    me.message.showPopo('', '<i18n key=METADATA_COMMON_REQUIRED_WARN>请选择数据</i18n>', 2000);
                    return;
                }
                Devp.comp.MessageBox.showConfirm("<i18n key=OP_YTSCYX_DELET_DETERMINE>确定要删除</i18n>", function() {
                    //向后台发送aax请求
                    Devp.Utils.ajaxRequest(me.delUrl, function(data) {
                        me.table.unmask();
                        // 错误信息
                        var errCode = JSON.parse(data.responseText).model.errorCode;
                        if (errCode == null) {
                            me.table.store.loadPage(1);
                            me.message.showPopo('', '<i18n key=OP_YTSCYX_DELET_SUCCESS>删除成功</i18n>', 2000);
                            //删除数据后禁用按钮
                            me.viewBtn.disable();
                            me.editBtn.disable();
                            me.delBtn.disable();
                        }
                    }, function() {
                        me.message.showPopo('', '<i18n key=OP_YTSCYX_DELET_FAIL>删除失败</i18n>', 2000);
                    }, 60000, true, {'vo.entity.identifier' : selected[0].data.entity.identifier});
                });
            }
        });
        me.viewBtn = Ext.create({
            xtype: 'button',
            iconCls: 'orgauth_view',
            disabled: true,
            text: '<i18n key=METADATA_COMMON_VIEW>查看</i18n>',
            handler: function () {
                var selModel = me.table.getSelectionModel();
                var selected = selModel.getSelection();
                if (Ext.isEmpty(selected)) {
                    me.message.showPopo('', '<i18n key=METADATA_COMMON_REQUIRED_WARN>请选择数据</i18n>', 2000);
                    return;
                }
                var win = me.createViewWindow(selected[0].data.entity.identifier);
                win.show();
            }
        });
        me.exportDataBtn = Ext.create({
            xtype: 'button',
            iconCls: 'icon_daochu',
            text: '<i18n key=METADATA_COMMON_EXPORT>导出</i18n>',
            handler: function () {
                var fileName = '<i18n key=xxx>xxx</i18n>';
                me.makeUpNameForExport(this, fileName, true, "entityId", 'startDate');
            }
        });
    },
    
    /**
     * 创建新增窗口
     */
    createAddWindow: function() {
        var me = this;
        //创建窗口
        var win = new Ext.Window({
            title: '<i18n key=METADATA_COMMON_NEW>新增</i18n>',
            layout: 'fit',
            width: 750,
            resizable : false,
            modal: true,
            closeAction: 'destroy',//点击右上角关闭按钮后会执行的操作;
            closable: true,//隐藏关闭按钮;
            draggable: true,//窗口可拖动;
            items: [{
                xtype: 'form',
                split: {size: 2},
                bodyPadding: 10,
                defaults: {
                    labelAlign: 'right',
                    labelWidth :150
                },
                layout: {
                    type: 'table',
                    columns: 2
                },
                items:[
                    {//主数据区块下拉框
                        xtype: 'mdmcombo',
                        allowBlank: false,
                        editable: false,
                        name: 'entity.wellboreId',
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_WELLBORE_NAME>井筒名</i18n>',
                        jsClass: 'Devp.dh.basedata.OperationBaseInfo',
                        mdmId: '60E1ABFED26E4F6E881844EBB8C1D867',//井筒主数据ID TODO 后期改为字典获取
                        onItemClick: function (view, record) {
                            var form = this.up('form');
                            if (record.data.masterDataCode == "WELLBORE") {
                                this.selectItem(record);
                            }
                            var me = this;
                            //带出区块
                            var response = $.ajax({
                                url: basepath + '/masterdata/wellbore/query_info.ajson',
                                data: {'vo.entity.identifier': record.data.entity.identifier},
                                cache: false,
                                async: false
                            });
                            var vo = $.parseJSON(response.responseText);
                            if (Ext.isEmpty(vo.model.errorCode)) {
                                if (vo.model.entity == null) {
                                    me.message.showPopo('', '无法获得指定数据,请与系统管理员联系.', 2000);
                                }
                                form.form.findField('projectName').setValue(vo.model.projectName);
                            }
                        },
                        listeners: {
                            render: function (obj) {
                                var font = document.createElement("font");
                                font.setAttribute("color", "red");
                                var redStar = document.createTextNode('*');
                                font.appendChild(redStar);
                                obj.el.dom.appendChild(font);
                            },
                            select:function(obj){
                                me.onlyValidate(obj,me);
                                obj.focus();
                            }
                        }
                    },
                    {
                        xtype: 'textfield',
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_PROJECT_NAME>油田</i18n>',
                        disabled: true,
                        name: 'projectName',
                        listeners: {
                            render: function (obj) {
                                var font = document.createElement("font");
                                font.setAttribute("color", "red");
                                var redStar = document.createTextNode('*');
                                font.appendChild(redStar);
                                obj.el.dom.appendChild(font);
                            }
                        }
                    },
                    {
                        xtype: 'combo',
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_RIG_NAME>修井机名称</i18n>',
                        allowBlank: false,
                        editable: false,
                        name: 'entity.rigId',
                        store:{
                            xtype:'store',
                            fields: [{name:'text', mapping:'entity.rigName'},{name:'value', mapping:'entity.identifier'}],
                            data : me.rigList
                        },
                        displayField: 'text',
                        valueField: 'value',
                        listeners: {
                            render: function (obj) {
                                var font = document.createElement("font");
                                font.setAttribute("color", "red");
                                var redStar = document.createTextNode('*');
                                font.appendChild(redStar);
                                obj.el.dom.appendChild(font);
                            },
                            select:function(obj){
                                me.onlyValidate(obj,me);
                                var form = this.up('form');
                                form.form.findField('companyName').setValue(obj.selection.data.companyName);
                                obj.focus();
                            }
                        }

                    },
                    {
                        xtype: 'textfield',
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_COMPANY_NAME>服务商</i18n>',
                        disabled: true,
                        name: 'companyName'
                    },
                   
                    {
                        xtype: 'datefield',
                        format: 'Y-m-d',
                        editable: false,
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_MOVE_DATE>设备搬上时间</i18n>',
                        name: 'entity.moveDate',
                        maxValue: me.createDate()
                    },
                    {
                        xtype: 'textfield',
                        name: 'entity.comments',
                        fieldLabel:'<i18n key=DH_BASEDATA_OPERATIONBASEINFO_COMMENTS>中文备注</i18n>'
                    }],
                fbar: ['->',
                    {
                        formBind: true,
                        disabled: true,
                        text: '<i18n key=METADATA_COMMON_SAVE>保存</i18n>',
                        handler: function() {
                            var postadata = {};
                            var forms = this.up('window').query('form');
                            forms[0].items.each(function(item, index, length) {
                                if(!Ext.isEmpty(item.getValue())){
                                    if(item.getValue().toString().length>0){
                                        postadata[item.getName()] = item.getValue();
                                    }
                                }
                            });
                            //向后台发送新增请求
                            Devp.Utils.ajaxRequest(me.addUrl, function(data) {
                                // 修改本地
                                var errCode = JSON.parse(data.responseText).model.errorCode;
                                if (errCode == null) {
                                    me.table.store.loadPage(1);
                                    me.message.showPopo('', '<i18n key=METADATA_COMMON_SAVE_SUCCESS>保存成功</i18n>', 2000);
                                    forms[0].up("window").close();
                                }
                            }, function() {
                                me.message.showPopo('', '<i18n key=METADATA_COMMON_SAVE_FAILURE>保存失败</i18n>', 2000);
                            }, 60000, true, postadata);
                        }},
                    {
                        text: '<i18n key=METADATA_COMMON_CANCEL>取消</i18n>',
                        handler: function() {
                            this.up("window").close();
                        }
                    }
                ]
            }]
        });
        return win;
    },
    
    /**
     * 创建编辑窗口
     */
    createEditWindow: function(id) {
        var me = this;
        //查询数据信息
        var vo = me.queryDataInfo(id);
        if(vo == null) return;
        var entity = vo.model.entity;
        //创建窗口
        var win = new Ext.Window({
            title: '<i18n key=METADATA_COMMON_NEW>编辑</i18n>',
            layout: 'fit',
            width: 750,
            modal: true,
            resizable : false,
            closeAction: 'destroy',//点击右上角关闭按钮后会执行的操作;
            closable: true,//隐藏关闭按钮;
            draggable: true,//窗口可拖动;
            items: [{
                xtype: 'form',
                split: {size: 2},
                bodyPadding: 10,
                defaults: {
                    labelAlign: 'right',
                    labelWidth :150
                },
                layout: {
                    type: 'table',
                    columns: 2
                },
                items:[
                    {//为了唯一性验证而增
                        xtype: 'textfield',
                        name: 'entity.wellboreId',
                        hidden: true,
                        disabled: true,
                        value: entity.wellboreId
                    },
                    {
                        xtype: 'textfield',
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_WELLBORE_NAME>井筒名</i18n>',
                        editable: false,
                        disabled: true,
                        value: vo.model.wellboreName,
                        listeners: {
                            render: function (obj) {
                                var font = document.createElement("font");
                                font.setAttribute("color", "red");
                                var redStar = document.createTextNode('*');
                                font.appendChild(redStar);
                                obj.el.dom.appendChild(font);
                            }
                        }
                    },
                    {
                        xtype: 'textfield',
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_PROJECT_NAME>油田</i18n>',
                        editable: false,
                        disabled: true,
                        value: vo.model.projectName,
                        listeners: {
                            render: function (obj) {
                                var font = document.createElement("font");
                                font.setAttribute("color", "red");
                                var redStar = document.createTextNode('*');
                                font.appendChild(redStar);
                                obj.el.dom.appendChild(font);
                            }
                        }
                    },
                    {
                        xtype: 'combo',
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_RIG_NAME>修井机名称</i18n>',
                        editable: false,
                        allowBlank: false,
                        name: 'entity.rigId',
                        value: entity.rigId,
                        store:{
                            xtype:'store',
                            fields: [{name:'text', mapping:'entity.rigName'},{name:'value', mapping:'entity.identifier'}],
                            data : me.rigList
                        },
                        displayField: 'text',
                        valueField: 'value',
                        listeners: {
                            render: function (obj) {
                                var font = document.createElement("font");
                                font.setAttribute("color", "red");
                                var redStar = document.createTextNode('*');
                                font.appendChild(redStar);
                                obj.el.dom.appendChild(font);
                            },
                            change: function(obj){
                                me.onlyValidate(obj,me);
                                var form = this.up('form');
                                form.form.findField('companyName').setValue(obj.selection.data.companyName);
                                obj.focus();
                            }
                        }

                    },
                    {
                        xtype: 'textfield',
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_COMPANY_NAME>服务商</i18n>',
                        disabled: true,
                        name: 'companyName',
                        value: vo.model.companyName
                    },                  
                    {
                        xtype: 'datefield',
                        format: 'Y-m-d',
                        editable: false,
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_MOVE_DATE>设备搬上时间</i18n>',
                        name: 'entity.moveDate',
                        value: entity.moveDate,
                        maxValue: me.createDate()
                    },
                    {
                        xtype: 'textfield',
                        name: 'entity.comments',
                        value: entity.comments,
                        fieldLabel:'<i18n key=DH_BASEDATA_OPERATIONBASEINFO_COMMENTS>中文备注</i18n>'
                    }
                ],
                fbar: ['->',
                    {
                        formBind: true,
                        disabled: true,
                        text: '<i18n key=METADATA_COMMON_SAVE>保存</i18n>',
                        handler: function() {
                            var postadata = {};
                            var forms = this.up('window').query('form');
                            forms[0].items.each(function(item, index, length) {
                                postadata[item.getName()] = item.getValue();
                            });
                            postadata['entity.identifier'] = id;
                            //向后台发送新增请求
                            Devp.Utils.ajaxRequest(me.editUrl, function(data) {
                                // 修改本地
                                var errCode = JSON.parse(data.responseText).model.errorCode;
                                if (errCode == null) {
                                    me.table.store.loadPage(1);
                                    me.message.showPopo('', '<i18n key=METADATA_COMMON_SAVE_SUCCESS>保存成功</i18n>', 2000);
                                    forms[0].up("window").close();
                                }
                            }, function() {
                                message.showPopo('', '<i18n key=METADATA_COMMON_SAVE_FAILURE>保存失败</i18n>', 2000);
                            }, 60000, true, postadata);
                        }},
                    {
                        text: '<i18n key=METADATA_COMMON_CANCEL>取消</i18n>',
                        handler: function() {
                            this.up("window").close();
                        }
                    }
                ]
            }]
        });
        return win;
    },
    
    /**
     * 创建查看窗口
     */
    createViewWindow: function(id) {
        var me = this;
        //查询数据信息
        var vo = me.queryDataInfo(id);
        if(vo == null) return;
        var entity=vo.model.entity;
        var win = new Ext.Window({
            title: '<i18n key=METADATA_COMMON_VIEW>查看</i18n>',
            resizable : false,
            layout: 'fit',
            width: 750,
            modal: true,
            closeAction: 'destroy',//点击右上角关闭按钮后会执行的操作;
            closable: true,//隐藏关闭按钮;
            draggable: true,//窗口可拖动;
            items: [{
                xtype: 'form',
                split: {size: 2},
                bodyPadding: 10,
                defaults: {
                    labelAlign: 'right',
                    labelWidth :150
                },
                layout: {
                    type: 'table',
                    columns: 2
                },
                items:[{
                        xtype: 'textfield',
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_WELLBORE_NAME>井筒名</i18n>',
                        readOnly: true,
                        value: vo.model.wellboreName
                    },
                    {
                        xtype: 'textfield',
                        readOnly: true,
                        value: vo.model.projectName,
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_PROJECT_NAME>油田</i18n>'
                    },
                    {
                        xtype: 'textfield',
                        readOnly: true,
                        value: vo.model.rigName,
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_RIG_NAME>修井机名称</i18n>'
                    },
                    {
                        xtype: 'textfield',
                        readOnly: true,
                        value: vo.model.companyName,
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_COMPANY_NAME>服务商</i18n>'
                    },
                    {
                        xtype: 'textfield',
                        readOnly: true,
                        value: vo.model.operationTypeName,
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_OPERATION_TYPE_ANME>作业类别</i18n>'
                    },
                    {
                        xtype: 'datefield',
                        format: 'Y-m-d',
                        readOnly: true,
                        value: entity.moveDate,
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_MOVE_DATE>设备搬上时间</i18n>'
                    },
                    {
                        xtype: 'datefield',
                        format: 'Y-m-d',
                        readOnly: true,
                        value: entity.startDate,
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_START_DATE>作业开始时间</i18n>'
                    },
                    {
                        xtype: 'datefield',
                        format: 'Y-m-d',
                        readOnly: true,
                        value: entity.endDate,
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_END_DATE>作业结束时间</i18n>'
                    },
                    {
                        xtype: 'textfield',
                        readOnly: true,
                        value: entity.costCode,
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_COST_CODE>费用编码</i18n>'
                    },
                    {
                        xtype: 'textfield',
                        readOnly: true,
                        value: entity.superior,
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_SUPERIOR>监督人</i18n>'
                    },
                    {
                        xtype: 'textfield',
                        readOnly: true,
                        value: entity.comments,
                        fieldLabel: '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_COMMENTS>中文备注</i18n>'
                    }
                ],
                fbar: ['->',
                    {
                        text: '<i18n key=METADATA_COMMON_CLOSE>关闭</i18n>',
                        handler: function() {
                            this.up("window").close();
                        }
                    }
                ]
            }]
        });
        return win;
    },
    
    /**
     * 查询数据信息
     */
    queryDataInfo: function(id) {
        var me = this;
        //获取详细信息
        var response = $.ajax({
            url: me.viewUrl,
            data: {'vo.entity.identifier':id},
            cache: false,
            async: false
        });
        var vo = $.parseJSON(response.responseText);
        if(Ext.isEmpty(vo.model.errorCode)) {
            if(vo.model.entity == null) {
                me.message.showPopo('', '<i18n key=METADATA_COMMON_NODATA_ERROR>无法获得指定数据,请与系统管理员联系.</i18n>', 2000);
                return null;
            }
            return vo;
        }
        return null;
    },
    
    /**
     * 唯一性验证
     * @param obj
     * @param me
     */
    onlyValidate:function(obj,me){
        //var wellboreId = obj.ownerCt.getValues()['entity.wellboreId'];
        var wellboreId = obj.ownerCt.query('[name=entity.wellboreId]')[0].getValue();
        var rigId = obj.ownerCt.query('[name=entity.rigId]')[0].getValue();
        var operationType = obj.ownerCt.query('[name=entity.operationType]')[0].getValue();
        var startDate = obj.ownerCt.query('[name=entity.startDate]')[0].getValue();
        if(!Ext.isEmpty(wellboreId)&&!Ext.isEmpty(rigId)&&!Ext.isEmpty(operationType)&&!Ext.isEmpty(startDate)){
            startDate = Ext.util.Format.date(startDate, 'Y-m-d');
            var response = $.ajax({
                url: me.viewUrl,
                data: {'entity.wellboreId':wellboreId, 'entity.rigId':rigId, 'entity.operationType':operationType, 'entity.startDate':startDate},
                cache: false,
                async: false
            });
            var res = $.parseJSON(response.responseText);
            if(!Ext.isEmpty(res.model.entity)&&!Ext.isEmpty(res.model.entity.identifier)){
                me.message.showPopo('', res.model.wellboreName + '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_WELLBORE>井筒</i18n>'
                    + res.model.rigName + '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_RIG>修井机</i18n>'+ res.model.operationTypeName + '<i18n key=DH_BASEDATA_OPERATIONBASEINFO_OPERATION_TYPE_ANME>作业类别</i18n>' +
                    startDate + '<i18n key=OP_DJSC_ONLY_EXIST_PROMPT_DATE>日期已存在，请编辑此数据</i18n>', 2000);
                obj.ownerCt.query('[name=entity.startDate]')[0].setValue(null);
            }
        }
    },

});
```
