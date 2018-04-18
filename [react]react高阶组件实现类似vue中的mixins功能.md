#### render函数惰性执行
使用 ++immutabe++ 中 ++is++ 方法判断被包装的组件 ++shouldComponentUpdate++ 周期中的props和state是否有变化来确定被包装的组件是否执行render函数

````js
import React, { Component } from 'react';
import { fromJS, is } from 'immutable';

const InertRender = function (WrappedComponent) {
    return class ImmutableRender extends Component {
        componentDidMount() {
            const wrapped = this.refs.wrapped;
            wrapped.shouldComponentUpdate = function(nextProps, nextState) {
                let stateChange = !is(fromJS(nextState), fromJS(wrapped.state));
                let propsChange = !is(fromJS(nextProps), fromJS(wrapped.props));

                if (stateChange || propsChange) return true;
                return false
            }
        }

        render() {
            return <WrappedComponent ref="wrapped" {...this.props} />
        }
    }
}

export default InertRender
````