title: 阿波罗PC自开发内容 
date: 2015-09-01 13:57:15
tags:
 - 工作
---

阿波罗PC开发
=============

1. 双向绑定
============

首先在需要双向绑定的类上面设置注解@linkedStateMixin。然后在构造函数内用this.setBiding('users')设置绑定的store(redux1.0后改名为reducer)对象，此对象需要在this.props中存在，同时要求this.props中存在dispatch对象（redux产生的）

    import linkedStateMixin from '../decorators/LinkedStateMixin.js'
    
    @linkedStateMixin
    export default class UserManager extends Component {
        constructor(props) {
            super(props);
            this.setBinding('users');
        }
         render() {               
                return (
                    
                        <Input
                            type='text'
                            valueLink={this.binding('nameOrId')}
                             />
                 );
        }
    }
在store(redux1.0后改名为reducer)创建的时候，需要用bindingStore方法创建。sotore中的初始对象如果需要绑定，需要先声明好，比如你要绑定下面store中的showUser.name这个属性，那么store中showUser必须要默认声明好showUser:{}，然后valueLink={this.binding("showUser,name")}

    import {UserTypes,MainTypes} from '../constants/ActionTypes'
    import bindingStore from '../lib/bindingStore'
    import Immutable from 'immutable'
    
    const initialState = Immutable.fromJS({
        users: [],
        totalCount: 0,
        showUser:{},
        userRoles:[],
        userPermissions:[],
        rolesToSelect:{
            roles:[]
        }
    });
    //TODO defend override redux,write more code
    export default bindingStore('users',initialState, {
        [UserTypes.LOAD_USERS]: (data, action) => {
            return data.merge(Immutable.fromJS(action.data));
        }
    })
    
2. 全局styles
============

react的css设置推崇inline-style,就是所有的组件或者dom对象的css直接设置在style={% raw -%}{{}}{%- endraw %}里。这样做的好处是可以利用js的逻辑能力，变化css,并且对组件的封装性加强。我们用publicStyle这个注解做全局的公用inline-style,用法如下。其中this.mergeStyle是publicStyle注进来的方法，其参数为更重要级的style，比如参数中有input对象，优先级比publicStyles里面的高，全局的inline-style写到publicStyle里面，如下面第二段代码。

    @publicStyle
    @linkedStateMixin
    export default
    class UserManager extends Component {
        constructor(props) {
            super(props);            
            this.styles = this.mergeStyle(this.getStyles());
        }
        
        render(){
            const styles = this.styles;
            return (
                <input type='text' style={styles.input}/>
            )
        }
        
        getStyles(){
            return {
                input:{
                    margin:10
                }
            }
        }
    }
    
publicStyle里面的代码是全局inline-style.   
    
    export default DecoratedComponent => {
        DecoratedComponent.prototype.publicStyles = function () {
            let styles = {
                paper: {
                    padding: 25
                },
                flexColumn: {
                    display: 'flex',
                    flexDirection: 'column',
                    flexWrap: 'nowrap',
                },
                flexRow: {
                    display: 'flex',
                    flexDirection: 'row',
                    flexWrap: 'nowrap',
                },
                flex: {
                    display: 'flex',
                    flexDirection: 'row',
                    flexWrap: 'nowrap'
                },
                searchFlex: {
                    display: 'flex',
                    flexDirection: 'row',
                    flexWrap: 'nowrap',
                    alignItems: 'center'
                }
            };
            return styles;
        }
    }
    
3. 自定义弹出框，普通弹出框的用法
=============================
1. 自定义弹出框
--------------
首先在constants里面的ModalTypes里面添加一个枚举：

    export const ModalTypes= {
    
        ERROR_INFO:'ERROR_INFO',
        DIALOG_EXAMPLE:'DIALOG_EXAMPLE'
    }

修改components下面的ModalRouter组件，添加刚才枚举所对应的组件。
自定义组件：

     import React, { Component, PropTypes } from 'react';
     import {Button,Input,Grid,Row,Col,Modal} from 'react-bootstrap';
     import PublicStyles from '../decorators/PublicStyles'
     
     @PublicStyles
     export default
     class DialogExample extends Component {
     
         static proTypes = {
     
         };
     
         render() {
             let styles = this.mergeStyle();
             return (
                 <div>
                     <Modal.Header closeButton onHide={this.props.onClose}>
                         <Modal.Title>
                           title
                         </Modal.Title>
     
                     </Modal.Header>
                     <Modal.Body>
                        sadfsadf
                     </Modal.Body>
                     <Modal.Footer>
                         <Button>test1</Button>
                     </Modal.Footer>
                 </div>
             );
         }
     }
     
修改ModalRouter.js:

    renderModal() {
            const {modalType} = this.props;
            switch (modalType) {
                case ModalTypes.ERROR_INFO:
                    return (
                        <ErrorInfo onClose={this.props.close} {...this.props}/>
                    );
                case ModalTypes.ADJUSTMENT_INSERT:
                    return (
                        <AdjustmentInsert onClose={this.props.close} {...this.props} />
                    )
                case ModalTypes.DIALOG_EXAMPLE:
                    return (
                        <DialogExample onClose={this.props.close} {...this.props} />
                    )
    
                default :
                    return;
            }
        }
        
调用自定义弹框：

    import * as MainActions from '../actions/MainActions.js'
    //构造函数
    constructor(props) {
            super(props);
            this.actions = bindActionCreators(MainActions, props.dispatch);
        }
    //调用语句
    this.actions.showAlert({modalType:ModalTypes.DIALOG_EXAMPLE});

如果是想用预制的普通文字弹出框直接调用：

    this.actions.showAlert({
            modalType:ModalTypes.ERROR_INFO,
            title: 'titleTest',
            info: 'contentTest',
            buttonFunc:[
                {
                    event: function (target) {
                        target.props.drawbackScheme(target.props.scheme, btn);
                    },
                    text: "确认",
                    style: "danger"
    
                },
                {
                    event: function (target) {
                        target.props.closeAlert()
                    },
                    text: "取消",
                    style: "warning"
    
                }
            ]
        });