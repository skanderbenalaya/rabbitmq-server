PROJECT = rabbitmq_management_agent
PROJECT_DESCRIPTION = RabbitMQ Management Agent
PROJECT_MOD = rabbit_mgmt_agent_app

# Make sure we know where the applications are located.
ROOT_DIR ?= $(realpath ../..)
APPS_DIR ?= $(realpath ..)
DEPS_DIR ?= $(realpath ../../deps)

define PROJECT_ENV
[
	    {rates_mode,        basic},
	    {sample_retention_policies,
	     %% List of {MaxAgeInSeconds, SampleEveryNSeconds}
	     [{global,   [{605, 5}, {3660, 60}, {29400, 600}, {86400, 1800}]},
	      {basic,    [{605, 5}, {3600, 60}]},
	      {detailed, [{605, 5}]}]}
	  ]
endef

define PROJECT_APP_EXTRA_KEYS
	{broker_version_requirements, []}
endef

TEST_DEPS = rabbitmq_ct_helpers rabbitmq_ct_client_helpers
LOCAL_DEPS += rabbit_common rabbit xmerl mnesia ranch ssl crypto public_key
DEP_EARLY_PLUGINS = $(PROJECT)/../rabbit_common/mk/rabbitmq-early-plugin.mk
DEP_PLUGINS = $(PROJECT)/../rabbit_common/mk/rabbitmq-plugin.mk

# FIXME: Use erlang.mk patched for RabbitMQ, while waiting for PRs to be
# reviewed and merged.

ERLANG_MK_REPO = https://github.com/rabbitmq/erlang.mk.git
ERLANG_MK_COMMIT = rabbitmq-tmp

include $(ROOT_DIR)/rabbitmq-components.mk
TEST_DEPS := $(filter-out rabbitmq_test,$(TEST_DEPS))
include $(ROOT_DIR)/erlang.mk

$(PROJECT).d::
	$(MAKE) -C $(APPS_DIR)/rabbitmq_cli app IS_APP=1
