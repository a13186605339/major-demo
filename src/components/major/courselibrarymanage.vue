<template>
  <div class="course-library-manage">
    <Button type="primary" @click="modal1 = true">新建课程</Button>
    <Button type="success">导入课程</Button>
    <div class="user-table">
      <Table :data="tableData1" :columns="tableColumns1" stripe></Table>
      <div style="margin: 10px;overflow: hidden">
        <div style="float: right;">
          <Page :total="100" :current="1" @on-change="changePage"></Page>
        </div>
      </div>
    </div>
    <Modal
      v-model="modal1"
      title="新增教职工"
      @on-ok="ok"
      @on-cancel="cancel">
      <Form  :label-width="80">
        <Form-item label="课程名称">
          <Input placeholder="请输入"></Input>
        </Form-item>
        <Form-item label="课程编码">
          <Input placeholder="请输入"></Input>
        </Form-item>
        <Form-item label="课程教材">
          <Input placeholder="请输入"></Input>
        </Form-item>
        <Form-item label="实训课时">
          <Input placeholder="请输入"></Input>
        </Form-item>
        <Form-item label="讲述课时">
          <Input placeholder="请输入"></Input>
        </Form-item>
        <Form-item label="学分">
          <Input-number :max="20" :min="1" ></Input-number>
        </Form-item>
        <Form-item label="承担院系">
          <Select placeholder="请选择">
            <Option value="beijing">计算机与软件学院</Option>
            <Option value="shanghai">能源与电气学院</Option>
            <Option value="shenzhen">国际教育学院</Option>
          </Select>
        </Form-item>
        <Form-item label="课程版本">
          <Radio-group>
            <Radio label="male">专科版</Radio>
            <Radio label="female">本科版</Radio>
          </Radio-group>
        </Form-item>
        <Form-item label="课程状态">
          <Radio-group>
            <Radio label="male">启用</Radio>
            <Radio label="female">停用</Radio>
          </Radio-group>
        </Form-item>
        <Form-item label="课程介绍">
          <Input type="textarea" :autosize="{minRows: 2,maxRows: 5}"
                 placeholder="请输入..."></Input>
        </Form-item>
        <Form-item label="课程备注">
          <Input type="textarea" :autosize="{minRows: 2,maxRows: 5}"
                 placeholder="请输入..."></Input>
        </Form-item>
      </Form>
    </Modal>
  </div>
</template>
<script>
  export default {
    data () {
      return {
        switchClassTrue: true,
        formValidate: {
          input: '',
          name: '',
          dep: '',
          type: '',
          title: '',
          number: '',
          status: '1'
        },
        ruleValidate: {
          name: [
            {required: true, message: '姓名不能为空', trigger: 'blur'}
          ],
          number: [
            {required: true, message: '姓名不能为空', trigger: 'blur'}
          ],
          mail: [
            {required: true, message: '邮箱不能为空', trigger: 'blur'},
            {type: 'email', message: '邮箱格式不正确', trigger: 'blur'}
          ],
          title: [
            {required: true, message: '请选择城市', trigger: 'change'}
          ],
          status: [
            {required: true, message: '请选择性别', trigger: 'change'}
          ],
          interest: [
            {required: true, type: 'array', min: 1, message: '至少选择一个爱好', trigger: 'change'},
            {type: 'array', max: 2, message: '最多选择两个爱好', trigger: 'change'}
          ],
          date: [
            {required: true, type: 'date', message: '请选择日期', trigger: 'change'}
          ],
          time: [
            {required: true, type: 'date', message: '请选择时间', trigger: 'change'}
          ],
          desc: [
            {required: true, message: '请输入个人介绍', trigger: 'blur'},
            {type: 'string', min: 20, message: '介绍不能少于20字', trigger: 'blur'}
          ]
        },
        modal1: false,
        tableData1: this.mockTableData1(),
        tableColumns1: [
          {
            title: '用户名称',
            key: 'name'
          },
          {
            title: '工号',
            key: 'status',
            render: (h, params) => {
              const row = params.row;
              const color = row.status === 1 ? 'blue' : row.status === 2 ? 'green' : 'red';
              const text = row.status === 1 ? '构建中' : row.status === 2 ? '构建完成' : '构建失败';

              return h('Tag', {
                props: {
                  type: 'dot',
                  color: color
                }
              }, text);
            }
          },
          {
            title: '类型',
            key: 'portrayal',
            render: (h, params) => {
              return h('Poptip', {
                props: {
                  trigger: 'hover',
                  title: params.row.portrayal.length + '个画像',
                  placement: 'bottom'
                }
              }, [
                h('Tag', params.row.portrayal.length),
                h('div', {
                  slot: 'content'
                }, [
                  h('ul', this.tableData1[params.index].portrayal.map(item => {
                    return h('li', {
                      style: {
                        textAlign: 'center',
                        padding: '4px'
                      }
                    }, item)
                  }))
                ])
              ]);
            }
          },
          {
            title: '职称',
            key: 'people',
            render: (h, params) => {
              return h('Poptip', {
                props: {
                  trigger: 'hover',
                  title: params.row.people.length + '个客群',
                  placement: 'bottom'
                }
              }, [
                h('Tag', params.row.people.length),
                h('div', {
                  slot: 'content'
                }, [
                  h('ul', this.tableData1[params.index].people.map(item => {
                    return h('li', {
                      style: {
                        textAlign: 'center',
                        padding: '4px'
                      }
                    }, item.n + '：' + item.c + '人')
                  }))
                ])
              ]);
            }
          },
          {
            title: '部门',
            key: 'time',
            render: (h, params) => {
              return h('div', '近' + params.row.time + '天');
            }
          },
          {
            title: '状态',
            key: 'update',
            render: (h, params) => {
              return h('div', this.formatDate(this.tableData1[params.index].update));
            }
          },
          {
            title: '操作',
            key: 'action',
            width: 150,
            align: 'center',
            render: (h, params) => {
              return h('div', [
                h('Button', {
                  props: {
                    type: 'primary',
                    size: 'small'
                  },
                  style: {
                    marginRight: '5px'
                  },
                  on: {
                    click: () => {
                      this.show(params.index)
                    }
                  }
                }, '查看'),
                h('Button', {
                  props: {
                    type: 'error',
                    size: 'small'
                  },
                  on: {
                    click: () => {
                      this.remove(params.index)
                    }
                  }
                }, '删除')
              ]);
            }
          }
        ]
      }
    },
    methods: {
      mockTableData1 () {
        let data = [];
        for (let i = 0; i < 5; i++) {
          data.push({
            name: '商圈' + Math.floor(Math.random() * 100 + 1),
            status: Math.floor(Math.random() * 3 + 1),
            portrayal: ['城市渗透', '人群迁移', '消费指数', '生活指数', '娱乐指数'],
            people: [
              {
                n: '客群' + Math.floor(Math.random() * 100 + 1),
                c: Math.floor(Math.random() * 1000000 + 100000)
              },
              {
                n: '客群' + Math.floor(Math.random() * 100 + 1),
                c: Math.floor(Math.random() * 1000000 + 100000)
              },
              {
                n: '客群' + Math.floor(Math.random() * 100 + 1),
                c: Math.floor(Math.random() * 1000000 + 100000)
              }
            ],
            time: Math.floor(Math.random() * 7 + 1),
            update: new Date()
          })
        }
        return data;
      },
      formatDate (date) {
        const y = date.getFullYear();
        let m = date.getMonth() + 1;
        m = m < 10 ? '0' + m : m;
        let d = date.getDate();
        d = d < 10 ? ('0' + d) : d;
        return y + '-' + m + '-' + d;
      },
      changePage () {
        // 这里直接更改了模拟的数据，真实使用场景应该从服务端获取数据
        this.tableData1 = this.mockTableData1();
      },
      getAllUser () {
        // 获取所有的用户
      },
      updateUser () {
        // 修改信息
      },
      deleteUser () {
        // 删除用户
      },

      ok () {
      },
      cancel () {
      }
    }
  }
</script>
<style lang="scss" rel="stylesheet/scss" type="text/scss">
  .course-library-manage {
    height: 100%;
    .user-table {
      height: 90%;
      overflow-y: auto;
      margin-top: 8px;
    }
  }
</style>
